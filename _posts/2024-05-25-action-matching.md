---
layout: post
title: "[Paper reading] Action Matching: Learning Stochastic Dynamics from Samples"
---
Learning dynamical systems from data is a central problem across physics, biology, and machine learning. The recent paper *"Action Matching: Learning Stochastic Dynamics from Samples"* (Neklyudov et al., 2023) proposes a method to recover time-evolving dynamics from static snapshots. Unlike traditional approaches requiring full trajectories, Action Matching (AM) operates on independently drawn samples from time-indexed distributions $\{p_t(x)\}_{t\in[0,1]}$. This opens the door to learning in domains where time series data is unavailable, such as single-cell biology or diffusion models.

This article offers an intuitive walkthrough of the motivation, formulation, and extensions of Action Matching. It also compares AM with Flow Matching (FM), clarifies the roles of distributions and potentials in dynamics, and interprets the algorithm’s behavior with mathematical precision.

### Problem Setup and Core Idea

We are given i.i.d. samples $\{x_i^t\}\sim p_t(x)$ at different time points $t\in[0,1]$. The goal is to learn a velocity field $v(x,t)$ such that the evolution of $p_t(x)$ satisfies the **continuity equation**:

\\[
\partial_t p_t(x) + \nabla\cdot\bigl(p_t(x)\,v(x,t)\bigr) = 0.
\\]

This equation ensures conservation of probability mass over time.

AM proposes to restrict $v(x,t)=\nabla s(x,t)$ and find a scalar potential $s$ that minimizes the total kinetic energy (action):

\\[
\mathcal{A}[v] = \frac12 \int_0^1 \int_{\mathbb{R}^d} \|v(x,t)\|^2\,p_t(x)\,dx\,dt
= \frac12 \int_0^1 \int \|\nabla s(x,t)\|^2\,p_t(x)\,dx\,dt.
\\]

This is rooted in the **principle of least action**: among all admissible flows satisfying the continuity equation, nature prefers the one minimizing energy.

### Understanding $p_t(x)$, $s_t(x)$, and $v_t(x)$

To intuitively grasp the method, consider:

- **$p_t(x)$**: probability density at time $t$. It describes the evolving mass distribution.
- **$s_t(x)$**: scalar potential. The “energy landscape” inducing motion.
- **$v_t(x)=\nabla s_t(x)$**: velocity field. Mass flows in the direction of the potential gradient.

This setup implies the dynamics are curl-free (i.e.\ $\nabla\times v=0$), consistent with conservative systems. The gradient assumption also reduces parameter complexity: we model a scalar $s$ rather than a full vector field.

Imagine $s_t(x)$ as a topographic map—particles slide along the gradient $\nabla s_t(x)$, and $p_t(x)$ changes as mass flows down this landscape.

### Comparing Action Matching and Flow Matching

**Flow Matching (FM)** and **Action Matching (AM)** both recover time-dependent vector fields $v(x,t)$ from static time-sliced data.

**Shared structure:**
- Access only to samples from $\{p_t(x)\}$, not trajectories.
- Learn $v(x,t)$ that induces valid evolution.
- No likelihood or adversarial training.

**Key differences:**
- FM assumes access to point pairs $(x_0,x_1)$ and minimizes:
  \\[
  \mathbb{E}_{t,x_0,x_1}\bigl[\|v(x(t),t)-\dot x(t)\|^2\bigr],
  \\]
  where $x(t)=(1-t)x_0+tx_1$.
- AM uses no explicit pairings and instead minimizes:
  \\[
  \mathcal{A}[v]=\frac12\int_0^1\!\int\|v(x,t)\|^2\,p_t(x)\,dx\,dt.
  \\]

Thus, FM does **local pointwise matching**, while AM performs **global energy minimization**.

### Method and Loss Function of Action Matching

To estimate $s$, AM constructs the **Action Matching loss**:

\\[
L_{AM}(s)=\mathcal{A}(s)-\mathcal{K}_{\text{AM}}(s),
\\]

where $\mathcal{K}_{\text{AM}}(s)$ is a cross-term derived via integration by parts:

\\[
\int_X\langle\nabla g,f\rangle\,dx
=\int_{\partial X} g\,f\cdot d\mathbf{n}
-\int_X g\,(\nabla\cdot f)\,dx.
\\]

Using the continuity equation $\nabla\cdot\bigl(q_t\nabla s_t^*\bigr)=-\partial_t q_t$, one gets:

\\[
\mathcal{K}_{\text{AM}}(s)
=-\int_0^1\!\int s_t(x)\,\partial_t q_t(x)\,dx\,dt.
\\]

Thus, $L_{AM}$ measures the kinetic energy gap between the model and the true process. Minimizing $L_{AM}$ maximizes a **variational lower bound** on the ground-truth action $\mathcal{A}(s^*)$.

### On Paths: AM vs. Wasserstein Geodesics

Figure 2 shows that AM’s inferred path need not coincide with the **Wasserstein-2 ($\mathcal{W}_2$) geodesic** between $p_0$ and $p_1$:

- **$\mathcal{W}_2$ geodesic** solves
  \\[
  \inf_{\gamma}\Bigl\{\int_0^1\!\int\|v\|^2p_t\,dx\,dt\quad\text{s.t.}\quad
  \partial_t p_t+\nabla\cdot(p_t v)=0\Bigr\},
  \\]
  yielding a time-varying, bespoke velocity field.
- **AM** fixes $v(x,t)=\nabla s(x,t)$ globally to fit all $p_t$, possibly deviating from the shortest OT path.

Hence, AM offers **dynamically consistent** but flexible transport, even when empirical marginals lie off the OT geodesic.

### Entropic Action Matching (eAM)

For **stochastic** dynamics, AM extends to **Entropic Action Matching**. eAM minimizes the expected energy of random paths plus an entropy penalty:

\\[
\mathbb{E}_{x_{0:T}}\Bigl[\int_0^1\!\tfrac12\|\dot x_t\|^2dt\Bigr]
+\tau\,\mathrm{KL}(\mathbb{P}\Vert\mathbb{W}),
\\]

where $\mathbb{P}$ is the learned path distribution and $\mathbb{W}$ is Brownian motion. This is the **Schrödinger bridge** formulation, making eAM robust to noise.

### Unbalanced Action Matching (uAM)

When mass is not conserved, uAM generalizes the continuity equation:

\\[
\partial_t q_t(x)+\nabla\cdot\bigl(q_t(x)\,v(x,t)\bigr)=r_t(x),
\\]

where $r_t(x)$ is a **mass source/sink**. The uAM loss becomes:

\\[
L_{uAM}(s,r)
=\mathcal{A}(s)-\mathcal{K}_{\text{AM}}(s)
+\lambda\int_0^1\!\int r_t(x)^2\,dx\,dt.
\\]

This handles scenarios like cell division/death, approximate generative sampling, and time-sliced scRNA-seq.

### Why Densities May Be Unnormalized in Diffusion Models

In diffusion models, intermediate $q_t(x)$ are often:

- **Unnormalized**: $q_t(x)\propto p_t(x)$,
- **Noisy**: due to SDE discretization,
- **Mass-leaky**: score models ignore normalization.

uAM’s source term $r_t(x)$ and penalty absorb these mass discrepancies, improving robustness in practice.

### Conclusion

Action Matching provides a principled framework for learning dynamics from unpaired snapshots, grounded in physics and optimal transport. Compared to Flow Matching, AM emphasizes **global energy efficiency** and **physical realism**. Its extensions eAM and uAM tackle stochasticity and mass imbalance, broadening applicability to modern generative modeling and scientific fields.

\\[
\boxed{\text{Action Matching = Least Action + Continuity Constraint + Sample-Based Optimization}}
\\]


---
layout: post
title: "[Paper reading] Action Matching: Learning Stochastic Dynamics from Samples"
---

Learning dynamical systems from data is a central problem across physics, biology, and machine learning. The recent paper **"Action Matching: Learning Stochastic Dynamics from Samples"** (Neklyudov et al., 2023) proposes a method to recover time-evolving dynamics from static snapshots. Unlike traditional approaches requiring full trajectories, Action Matching (AM) operates on independently drawn samples from time-indexed distributions $\{p_t(x)\}_{t\in[0,1]}$. This opens the door to learning in domains where time series data is unavailable, such as single-cell biology or diffusion models.

This article offers an intuitive walkthrough of the motivation, formulation, and extensions of Action Matching. It also compares AM with Flow Matching (FM), clarifies the roles of distributions and potentials in dynamics, and interprets the algorithm’s behavior with mathematical precision.

## Problem Setup and Core Idea

We are given i.i.d. samples $\{x_i^t\}\sim p_t(x)$ at different time points $t\in[0,1]$. The goal is to learn a velocity field $v(x,t)$ such that the evolution of $p_t(x)$ satisfies the **continuity equation**:

\\[
\partial_t p_t(x) + \nabla\cdot\bigl(p_t(x)\,v(x,t)\bigr)=0.
\\]

This equation ensures conservation of probability mass over time.

AM proposes to restrict $v(x,t)=\nabla s(x,t)$ and find a scalar potential $s$ that minimizes the total kinetic energy (action):

\\[
\mathcal{A}[v] = \frac12 \int_0^1 \int_{\mathbb{R}^d} \|v(x,t)\|^2\,p_t(x)\,dx\,dt
= \frac12 \int_0^1 \int \|\nabla s(x,t)\|^2\,p_t(x)\,dx\,dt.
\\]

This is rooted in the **principle of least action**: among all admissible flows satisfying the continuity equation, nature prefers the one minimizing energy.

## Understanding $p_t(x)$, $s_t(x)$, and $v_t(x)$

- **$p_t(x)$**: probability density at time $t$. It describes where mass concentrates.
- **$s_t(x)$**: scalar potential (a “height map” or energy landscape) whose gradient drives motion.
- **$v_t(x)=\nabla s_t(x)$**: velocity field; mass flows along the potential gradient.

This implies the flow is **curl-free** ($\nabla\times v=0$), consistent with conservative systems. Modeling $v$ via a scalar $s$ reduces parameter complexity.

## Comparing Action Matching and Flow Matching

**Flow Matching (FM)** and **Action Matching (AM)** both learn $v(x,t)$ from unpaired time-sliced data.

**Shared:** access only to marginals $p_t$, no trajectories, no adversarial training.

**FM** minimizes local velocity mismatch:

\\[
\mathbb{E}_{t,x_0,x_1}\bigl[\|v(x(t),t)-\dot x(t)\|^2\bigr],
\\]

with $x(t)=(1-t)x_0+tx_1$.

**AM** minimizes global kinetic energy:

\\[
\mathcal{A}[v]=\frac12\int_0^1\!\int\|v(x,t)\|^2\,p_t(x)\,dx\,dt.
\\]

## Key Mathematical Tools

### Divergence Theorem (Gauss’s Theorem)

\\[
\int_X (\nabla\cdot F)\,dx = \int_{\partial X} F\cdot d\mathbf{n}.
\\]

*Intuition:* Divergence measures net outflow per unit volume; integrating over a region gives total flux through its boundary.

### Product Rule for Divergence

\\[
\nabla\cdot(g\,f)=\langle\nabla g, f\rangle + g\,(\nabla\cdot f).
\\]

*Intuition:* Change in the product $g\,f$ comes from both $g$ varying along $f$ and divergence of $f$ weighted by $g$.

### Integration by Parts for Divergence

\\[
\int_X \langle\nabla g, f\rangle\,dx
= \int_{\partial X} g\,f\cdot d\mathbf{n} - \int_X g\,(\nabla\cdot f)\,dx.
\\]

*Intuition:* Transfers a derivative from $g$ to $f$, producing a boundary term minus a volume term.

## Method and Loss Function of Action Matching

AM defines the loss:

\\[
L_{AM}(s)=\mathcal{A}(s)-\mathcal{K}_{\mathrm{AM}}(s),
\\]

where the cross-term $\mathcal{K}_{\mathrm{AM}}(s)$ is obtained by applying the above IBP to $f=q_t\nabla s_t^*$ and $g=s_t$, then using

\\[
\nabla\cdot(q_t
abla s_t^*)=-\partial_t q_t
\\]

to yield:

\\[
\mathcal{K}_{\mathrm{AM}}(s)=-\int_0^1\!\int s_t(x)\,\partial_t q_t(x)\,dx\,dt.
\\]

Minimizing $L_{AM}$ corresponds to maximizing a **variational lower bound** on the true action $\mathcal{A}(s^*)$.

## On Paths: AM vs. Wasserstein Geodesics

Figure 2 shows AM’s path need not match the **Wasserstein-2 ($\mathcal{W}_2$) geodesic** between $p_0,p_1$, which solves:

\\[
\inf_{\{p_t,v\}}\Bigl\{\int_0^1\!\int\|v\|^2\,p_t\,dx\,dt : \partial_t p_t+\nabla\cdot(p_t v)=0\Bigr\}.
\\]

AM fixes $v=\nabla s$ globally, trading shortest path for modeling flexibility.

## Entropic Action Matching (eAM)

For stochastic dynamics, eAM minimizes:

\\[
\mathbb{E}_{x_{0:T}}\Bigl[\int_0^1\!\tfrac12\|\dot x_t\|^2dt\Bigr]
+\tau\,\mathrm{KL}(\mathbb{P}\Vert\mathbb{W}).
\\]

This links to the **Schrödinger bridge** and adds an entropy penalty.

## Unbalanced Action Matching (uAM)

When mass varies, uAM generalizes:

\\[
\partial_t q_t+\nabla\cdot(q_t v)=r_t,
\\]

with source $r_t(x)$, and loss:

\\[
L_{uAM}(s,r)=\mathcal{A}(s)-\mathcal{K}_{\mathrm{AM}}(s)
+\lambda\int_0^1\!\int r_t(x)^2\,dx\,dt.
\\]

This handles birth/death, sampling errors, and unnormalized marginals.

## Why Densities May Be Unnormalized in Diffusion Models

Intermediate $q_t(x)$ often are unnormalized, noisy, or lose mass due to discretization and score approximations. uAM’s source term $r_t(x)$ absorbs these discrepancies.

## Conclusion

Action Matching provides a physics-grounded framework for learning dynamics from static snapshots, emphasizing global energy minimization under continuity. Compared to FM’s local matching, AM focuses on physical realism. Extensions eAM and uAM address stochasticity and mass imbalance, making AM versatile for modern generative modeling and scientific applications.

\\[
\boxed{\text{Action Matching = Least Action + Continuity + Sample-Based Optimization}}\\]

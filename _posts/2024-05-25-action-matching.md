---
layout: post
title: "Paper reading: Action Matching: Learning Stochastic Dynamics from Samples"
---

Learning dynamical systems from data is a central problem across physics, biology, and machine learning. 
The paper **"Action Matching: Learning Stochastic Dynamics from Samples"** (Neklyudov et al., 2023) proposes a method to recover time-evolving dynamics from static snapshots. Unlike traditional approaches requiring full trajectories, Action Matching (AM) operates on independently drawn samples from time-indexed distributions $\{p_t(x)\}_{t\in[0,1]}$. This opens the door to learning in domains where time series data is unavailable, such as single-cell biology or diffusion models.

## Problem Setup and Core Idea

We are given i.i.d. samples $\{x_i^t\}\sim p_t(x)$ at different time points $t\in[0,1]$. The goal is to learn a velocity field $v(x,t)$ such that the evolution of $p_t(x)$ satisfies the **continuity equation**:

\\[
\partial_t p_t(x) + \nabla\cdot\bigl(p_t(x)\,v(x,t)\bigr)=0.
\\]

This equation ensures conservation of probability mass over time.

AM proposes to restrict $v(x,t)=\nabla s(x,t)$ and find a scalar potential $s$ that minimizes the total kinetic energy (action):

\\[
\mathcal{A}[v] = \frac12 \int_{0}^{1} \int_{\mathbb{R}^d} \|v(x,t)\|^2\,p_t(x)\,dx\,dt
= \frac12 \int_{0}^{1} \int \|\nabla s(x,t)\|^2\,p_t(x)\,dx\,dt.
\\]

This is rooted in the **principle of least action**: among all admissible flows satisfying the continuity equation, nature prefers the one minimizing energy.

## $p_t$, $s_t$, and $v_t$

- **$p_t(x)$**: probability density at time $t$. It describes where mass concentrates.
- **$s_t(x)$**: scalar potential (a “height map” or energy landscape) whose gradient drives motion.
- **$v_t(x)=\nabla s_t(x)$**: velocity field; mass flows along the potential gradient.

Conceptually, we begin with $s$; this choice automatically specifies $v$, and from that $p$ is derived.
In Monge-Ampère or Kantorovich theory,
$s$ is a convex potential whose gradient gives the optimal map from $p_0$ to $p_1$.
In practice, modeling $v$ via a scalar $s$ reduces parameter complexity.

## Vs. Flow Matching

Flow Matching (FM) and Action Matching (AM) both learn $v(x,t)$ from unpaired time-sliced data.
Both access only to marginals $p_t$, no trajectories, no adversarial training,
but the differences are as follows.

**FM** minimizes local velocity mismatch:

\\[
\mathop{\mathbb{E}}\_{t,x_0,x_1}\bigl[\|v(x(t),t)-\dot x(t)\|^2\bigr],
\\]

with $x(t)=(1-t)x_0+tx_1$.

**AM** minimizes global kinetic energy:

\\[
\mathcal{A}[v]=\frac12\int_{0}^{1} \int\|v(x,t)\|^2\,p_t(x)\,dx\,dt,
\\]
which corresponds to the Benamou–Brenier action.

### Curl-free
The continuity equation by itself merely enforces mass conservation;
It says nothing about whether $v$ has curl or not. 
You can satisfy mass conservation with flow fields that swirl and vortex just fine.

The no curl ($\nabla\times v=0$) is guaranteed by the kinetic-energy minimization.
The AM  selects the curl-free (gradient) part as optimal, *because any rotational component only wastes energy.*
This can be explained by the Helmholtz decomposition.

## Objective Function

Let $s_t^\*$ be the ground-trueth action.
Like score matching, we obtain $s_t$ that approximates $s_t^\*$ by minimizing
\\[
\mathrm{ACTION\text{-}GAP}(s, s^\*)
:=
\frac{1}{2}
\int_{0}^{1}
\mathop{\mathbb{E}}\_{x \sim q_{t}}
\bigl\|\nabla s_t(x)\;-\;\nabla s_t^\* (x)\bigr\|^2
\,dt.
\\]
We can decompose it as
\\[
\mathrm{ACTION\text{-}GAP}\bigl(s_t, s_t^\* \bigr)
= K + \mathcal{L}\_{\mathrm{AM}}(s_t),
\\]
where $\mathcal{L}\_{\mathrm{AM}}(s)$ is the Action Matching objective, which we minimize:

\\[
\mathcal{L}\_{\mathrm{AM}}(s)
:=
\mathop{\mathbb{E}}\_{x \sim q_0}\bigl[s_0(x)\bigr] - \mathop{\mathbb{E}}\_{x \sim q_1}\bigl[s_1(x)\bigr]
+
\int_{0}^{1}
\mathop{\mathbb{E}}\_{x \sim q_t} \Bigl[\tfrac{1}{2}\,\|\nabla s_t(x)\|^2
+\tfrac{\partial }{\partial t}s_t(x)\Bigr]
\,dt.
<!--
-->
\\]

Minimizing $\mathcal{L}_{\mathrm{AM}}$ corresponds to maximizing a **variational lower bound** on the true action $\mathcal{A}(s^\*)$.

**Mathematical tools** to derive the above is as follows:

*Divergence Theorem (Gauss’s Theorem).*

\\[
\int_X (\nabla\cdot F)\,dx = \int_{\partial X} F\cdot d\mathbf{n}.
\\]

Intuition: Divergence measures net outflow per unit volume; integrating over a region gives total flux through its boundary.

*Product Rule for Divergence.*

\\[
\nabla\cdot(g\,f)=\langle\nabla g, f\rangle + g\,(\nabla\cdot f).
\\]

Intuition: Change in the product $g\,f$ comes from both $g$ varying along $f$ and divergence of $f$ weighted by $g$.

*Integration by Parts for Divergence.*

\\[
\int_X \langle\nabla g, f\rangle\,dx
= \int_{\partial X} g\,f\cdot d\mathbf{n} - \int_X g\,(\nabla\cdot f)\,dx.
\\]

*Intuition:* Transfers a derivative from $g$ to $f$, producing a boundary term minus a volume term.


## Vs. Wasserstein Geodesics

The AM’s path need not match the **Wasserstein-2 ($\mathcal{W}\_2$) geodesic** between $p_0,p_1$.
AM fixes $v=\nabla s$ globally, trading shortest path for modeling flexibility.

## Entropic Action Matching (eAM)
In standard Action Matching, we assume the system evolves via a velocity field that deterministically moves particles along paths that minimize total kinetic energy.
But many real-world systems — especially in biology, physics, and ML — are stochastic:
- Particles or agents diffuse randomly (e.g., Brownian motion, gene expression noise),
- The system's behavior includes uncertainty, even if the macroscopic distribution is known.

So, deterministic paths aren't enough to capture this — we need to model distributions over paths.
Instead of minimizing just kinetic energy of deterministic paths,
minimize the expected kinetic energy over stochastic paths,
plus a penalty on randomness (i.e., entropy regularization).
This is deeply inspired by the Schrödinger bridge problem, which is a stochastic version of optimal transport.

For stochastic dynamics, eAM minimizes:

\\[
\mathop{\mathbb{E}}\_{x_{0:T}}\Bigl[\int_{0}^{1} \tfrac12\|\dot x_t\|^2dt\Bigr]
+\tau\,\mathrm{KL}(\mathbb{P}\Vert\mathcal{W}).
\\]
The second term is the entropy penalty, which prevents the model from just pushing everything into deterministic paths and forces a smooth, stochastic interpolation.

## Unbalanced Action Matching (uAM)
Standard Action Matching (and even Entropic AM) assumes mass conservation:
\\[
\int q_t(x)dx = 1
\quad \text{for all } t \in [0,1].
\\]
But in practice:
- The forward process may push probability into very diffuse tails (low density, hard to sample),
- The reverse process may be approximated imperfectly by a neural network,
- During training, you often only approximate or reweight 
 $p_t(x)$ using samples from $p_0(x)$ or $p_1(x)$.
- Or you're using unnormalized intermediate distributions, e.g., through importance weighting or score matching.

So intermediate densities $p_t(x)$
used in learning or sampling may not strictly integrate to 1 — they may gain or lose mass.
Indeed, in score-based diffusion models,
you may not know the exact normalization of $p_t(x)$
at intermediate times — only its shape (i.e., unnormalized density).
So effectively, you're working with an unnormalized proxy.

When mass varies, uAM 
introduces the source term $r_t(x)$ that adjusts for the “extra” or “missing” probability mass along the path — like a buffer zone for approximation errors as

\\[
\partial_t q_t+\nabla\cdot(q_t v)=r_t,
\\]

with source $r_t(x)$, and loss:

\\[
L_{uAM}(s,r)=\mathcal{A}(s)-\mathcal{K}\_{AM}(s) +\lambda \int_{0}^{1} \int r_{t}(x)^2 dx dt.
\\]

This handles birth/death, sampling errors, and unnormalized marginals.

## Conclusion

Action Matching provides a physics-grounded framework for learning dynamics from static snapshots, emphasizing global energy minimization under continuity. Compared to FM’s local matching, AM focuses on physical realism. Extensions eAM and uAM address stochasticity and mass imbalance, making AM versatile for modern generative modeling and scientific applications.

\\[
\boxed{\text{Action Matching = Least Action + Continuity + Sample-Based Optimization}}\\]

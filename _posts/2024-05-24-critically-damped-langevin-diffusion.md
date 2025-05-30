---
layout: post
title: "Paper reading: \"Score-Based Generative Modeling with Critically-Damped Langevin Diffusion\""
---

This post aims to give an intuitive walk-through of the **Critically-Damped Langevin Diffusion (CLD)** approach to score-based generative modeling.
We’ll build up from first principles—mass, forces, and noise—to show how adding momentum and tuning damping can dramatically speed up sampling.

## Overdamped vs. Underdamped Langevin: Why Momentum?

We consider a stochastic process in a quadratic potential $U(x) = \frac12 \beta \|x\|^2$.

**Overdamped Langevin (First-Order, No Momentum)**
\\[
\mathrm{d}x_t = -\nabla U(x_t)\,\mathrm{d}t + \sqrt{2}\,\mathrm{d}W_t
\\]
Here the particle has no inertia: friction immediately damps velocity.  Sampling moves by local Gaussian noise steps (a random walk).

**Underdamped Langevin (Second-Order, With Momentum)**
\\[
M\,\ddot x_t = -\nabla U(x_t) - \Gamma\,\dot x_t + \xi(t)
\\]
The particle has mass $M$, so inertia carries it forward.  Introduce velocity $v_t = \dot x_t$ and write:
\\[
  \mathrm{d}x_t = v_t\,\mathrm{d}t, \\
\\]
\\[
  M\,\mathrm{d}v_t = -\nabla U(x_t)\,\mathrm{d}t - \Gamma\,v_t\,\mathrm{d}t + \xi(t)
\\]
Momentum correlates successive updates, letting you coast through valleys and over barriers, thus mixing much faster.
Without momentum, sampling jitters locally; with momentum, it glides.

## Langevin SDE

\\[
M\,\ddot x = F_{\rm cons} + F_{\rm fric} + F_{\rm noise}
\\]
\\[
F_{\rm cons} = -\nabla U(x)
\\]
\\[
F_{\rm fric} = -\Gamma\,\dot x
\\]
\\[
F_{\rm noise} = \xi(t)
\\]
Hence,
\\[
M\,\ddot x = -\nabla U(x) - \Gamma\,\dot x + \xi(t)
\\]

Assume $\xi(t) = \sigma\,\eta(t)$ with $\mathbb{E}[\eta(t)\eta(t')] = \delta(t - t')$.
Velocity OU Process:
\\[
M\,\mathrm{d}v_t = -\Gamma\,v_t\,\mathrm{d}t + \sigma\,\mathrm{d}W_t
\\]
Stationary variance:
\\[
\mathbb{E}[v_t^2] = \frac{\sigma^2}{2\,\Gamma\,M}
\\]
Equipartition at temperature $T$:
\\[
\mathbb{E}[v^2] = \frac{k_B T}{M}
\quad\Longrightarrow\quad
\sigma^2 = 2\,\Gamma\,k_B T
\\]
White noise form: if $\mathrm{d}W_t=\eta(t)\,\mathrm{d}t$, then:
\\[
\xi(t)=\sigma\eta(t)=\sqrt{2\,\Gamma\,k_B T}\;\eta(t)
\\]
This enforces the fluctuation–dissipation theorem—correct equilibrium sampling.

##First-Order System in $(x,v)$ & Linear SDE
With $v_t=\dot x_t$:
\\[
  \mathrm{d}x_t = v_t\,\mathrm{d}t,\\
\\]
\\[
  M\,\mathrm{d}v_t = -\nabla U(x_t)\,\mathrm{d}t - \Gamma\,v_t\,\mathrm{d}t + \sqrt{2\,\Gamma\,k_B T}\,\mathrm{d}W_t
\\]
Pack into $u_t=(x_t,v_t)$:
\\[
\mathrm{d}u_t = A\,u_t\,\mathrm{d}t + B\,\mathrm{d}W_t
\\]
where
\\[
A=\begin{pmatrix}0 & I/M \\\\ -\beta & -\Gamma/M\end{pmatrix},
\quad
B=\begin{pmatrix}0 \\\\ \sqrt{2\,\Gamma\,k_B T}\end{pmatrix}
\\]

## Critical Damping: $\Gamma^2 = 4M$ via Characteristic Equation
Noise-free oscillator:
\\[
M\,\ddot x + \Gamma\,\dot x + \beta\,x = 0
\\]
Ansatz $x(t)=e^{\lambda t}$ gives:
\\[
M\lambda^2 + \Gamma\lambda + \beta = 0
\\]
For a double root: $\Gamma^2 - 4M\beta = 0 \implies \Gamma^2 = 4M\beta$.  Setting $\beta=1$ yields $\Gamma^2 = 4M$.
Critical damping removes oscillations but preserves maximal inertia for the fastest return to equilibrium.

## Score Matching Needs Only the $v$-Score
The joint density factorizes: $p_t(x,v)=p_t(x)\,p_t(v\mid x)$.  The score is:
\\[
\nabla_{(x,v)}\log p_t = (\nabla_x,\nabla_v) = (\*,\nabla_v\log p_t(v\mid x))
\\]
In the reverse-time ODE, the diffusion matrix zeros out the $x$-component, so we only learn:
\\[
s_\theta(x,v,t) \approx \nabla_v\log p_t(v\mid x)
\\]
We implement this via the $ε$-prediction trick: the network predicts the injected velocity noise component ε.

## Practical Takeaways 
- Inertia (momentum) turns local jiggles into ballistic motion—drastically faster mixing.
- Critical damping ($\Gamma^2=4M$) is the sweet spot: no oscillations, maximum inertia.
- Simplified score: only the velocity channel requires denoising.

By blending classical physics with modern deep learning, CLD achieves state-of-the-art sampling efficiency in score-based generative models.

---
layout: post
title: "Gaussian Shift Bound"
---
Another adversarial machine learning paper ["Certified Adversarial Robustness via Randomized Smoothing"](https://arxiv.org/pdf/1902.02918)
builds a theory based on
the notion of [the half space]({{ site.baseurl }}{% post_url 2022-01-26-gaussian-isoperimetric-inequality %}).
The paper introduced a technique referred to as *randomized smoothing* that transforms any classifier into a new one that admits a provable $\ell\_2$ robustness certificate. 
Its core relies on the Gaussian shift bound (a form of the Neyman–Pearson lemma for mean‑shifted Gaussians). 

## Gaussian Shift Bound


Let

\\[
X \sim \mathcal{N}(\mu,\sigma^2 I),\quad
Y \sim \mathcal{N}(\mu + \delta,\sigma^2 I).
\\]

Fix a measurable set $S \subset \mathbb{R}^d$ with

\\[
P\_X(S) = \Pr[X \in S] = q.
\\]

We seek

\\[
\inf\_{S: P\_X(S)=q} P\_Y(S)
\quad\text{and}\quad
\sup\_{S: P\_X(S)=q} P\_Y(S).
\\]

### Neyman–Pearson setup

We set up a hypothesis test between

\\[
H\_0: Z \sim p\_0 = \mathcal{N}(\mu,\sigma^2 I),
\quad
H\_1: Z \sim p\_1 = \mathcal{N}(\mu + \delta,\sigma^2 I).
\\]

The likelihood ratio is

\\[
\Lambda(z) = \frac{p\_1(z)}{p\_0(z)}
= \exp\Bigl(\frac{\delta^\top (z-\mu)}{\sigma^2} - \frac{\|\delta\|^2}{2\sigma^2}\Bigr),
\\]

which is monotone in $\delta^\top z$. By Neyman–Pearson, the extremal sets are of the form $\{\Lambda(z)\le\tau\}$ or $\{\Lambda(z)\ge\tau\}$, i.e.\ half‑spaces orthogonal to $\delta$.

### Reduction to 1‑D

Project onto $u = \delta / \|\delta\|\_2$, let $T = u^\top Z$. Then under $H\_0$, $T\sim \mathcal{N}(u^\top\mu,\sigma^2)$; under $H\_1$, $T\sim \mathcal{N}(u^\top\mu + \|\delta\|\_2,\sigma^2)$. All orthogonal directions integrate out identically.

#### Minimum case (infimum)

Choose $S\_{\min} = \{T \le t^\star\}$ so that $P\_{H\_0}(T \le t^\star)=q$. Then

\\[
t^\star = u^\top\mu + \sigma\,\Phi^{-1}(q),
\\]

and

\\[
\inf\_{S: P\_X(S)=q} P\_Y(S)
= P\_{H\_1}(T \le t^\star)
= \Phi\Bigl(\Phi^{-1}(q) - \frac{\|\delta\|\_2}{\sigma}\Bigr).
\\]

#### Maximum case (supremum)

Choose $S\_{\max} = \{T \ge t\_{\max}\}$ so that $P\_{H\_0}(T \ge t\_{\max})=q$. Then $t\_{\max} = u^\top\mu + \sigma\,\Phi^{-1}(1-q)$, and

\\[
\sup\_{S: P\_X(S)=q} P\_Y(S)
= P\_{H\_1}(T \ge t\_{\max})
= \Phi\Bigl(\Phi^{-1}(q) + \frac{\|\delta\|\_2}{\sigma}\Bigr).
\\]

## 2. Deriving Cohen et al.’s Theorem

Define a base classifier $f$ and its smoothed version

\\[
g(x) = \arg\max\_c \Pr\_{\varepsilon\sim \mathcal{N}(0,\sigma^2 I)}[f(x+\varepsilon)=c].
\\]

Let

\\[
p\_A = \Pr[f(x+\varepsilon)=c\_A],
\quad
p\_B = \max\_{B\neq A} \Pr[f(x+\varepsilon)=c\_B],
\\]

and consider any perturbation $\delta$ with $\|\delta\|\_2 = r$. Applying the above bounds to the regions $E\_A = \{z: f(z)=c\_A\}$ and $E\_B$ yields:

\\[
\Pr[f(x+\delta+\varepsilon)=c\_A]
\;\ge\;
\Phi\Bigl(\Phi^{-1}(p\_A) - \tfrac{r}{\sigma}\Bigr),
\quad
\Pr[f(x+\delta+\varepsilon)=c\_B]
\;\le\;
\Phi\Bigl(\Phi^{-1}(p\_B) + \tfrac{r}{\sigma}\Bigr).
\\]

Requiring the lower bound for $c\_A$ to exceed the upper bound for all other classes gives

\\[
\Phi\Bigl(\Phi^{-1}(p\_A) - \tfrac{r}{\sigma}\Bigr)
\>
\Phi\Bigl(\Phi^{-1}(p\_B) + \tfrac{r}{\sigma}\Bigr)
\quad\Longrightarrow\quad
r < \frac{\sigma}{2}\bigl(\Phi^{-1}(p\_A) - \Phi^{-1}(p\_B)\bigr).
\\]

This is exactly the certified radius in Cohen et al.’s main theorem.

## Conclusion

We have seen how the Gaussian shift bound—driven by the Neyman–Pearson lemma and reduction to one dimension—directly yields the dimension‑agnostic $\ell\_2$ certificate of randomized smoothing. This elegant result underpins many extensions to anisotropic noise, other norms, and detection strategies.


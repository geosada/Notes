---
layout: post
title: "Chernoff Bound"
---
What is concentration?
At the beginning of 
[Boucheron's book](https://www.hse.ru/data/2016/11/24/1113029206/Concentration%20inequalities.pdf) and
[lecture series](https://www.youtube.com/watch?v=NzMKIXsXJOE) by Sudeep Kamath,
it is explained by the following quotes from Talagrand:
*A random variable that **smoothly** depends on the influence of many independent
random variables satisfies Chernoff type bounds and is essentially constant.*
Letting $f: \mathbb{R}^{n} \rightarrow \mathbb{R}^{1}$ as $Z = f(x_{1}, x_{2}, \cdots, x_{n})$ for independent variables $x_{1}, x_{2}, \cdots, x_{n}$,
what the concentration means is that
\\[
    \mathbb{P}\\{ |Z - \mathop{\mathbb{E}} \left \lbrack Z \right \rbrack | \leq t\\} \label{eq:1}\tag{1}
\\]
is small.

![Concentration]({{site.baseurl}}/img/Chernoff/fig_concentration.png){: .centered width="500" }

## Sub-Gaussianity and Outline

As we will see, $  \mathbb{P}\\{ |Z - \mathop{\mathbb{E}} \left \lbrack Z \right \rbrack | \leq t\\} $ is bounded 
by the logarithm of the moment generating function,
\\[
    \psi_{Z - \mathop{\mathbb{E}}\left \lbrack Z \right \rbrack }(\lambda) 
    =
    \log \mathop{\mathbb{E}} \exp \left (\lambda (Z - \mathop{\mathbb{E}} \left \lbrack Z \right \rbrack) \right )
    \label{eq:2}\tag{2}
\\]
$\forall \lambda \geq 0$,
and thus our interest is to suppress $\psi_{Z - \mathop{\mathbb{E}}\left \lbrack Z \right \rbrack }(\lambda)$ so that 
Eq.$\,$($\ref{eq:1}$) is sufficiently small, i.e.,
$Z$ is concentrated arround $\mathop{\mathbb{E}} \left \lbrack Z \right \rbrack$.
We know that Gaussian density is concentrated on the mean and rapidly decreases as it spreads toward the tail.
This is a property of the Gaussian, but by controlling $\psi_{Z - \mathop{\mathbb{E}}\left \lbrack Z \right \rbrack }(\lambda)$,
we can have a Gaussian-like concentration even for variables $Z=f(X)$ that do not follow the Gaussian.
In particular, if 
\\[
    \psi_{Z - \mathop{\mathbb{E}}\left \lbrack Z \right \rbrack } \leq \frac{\lambda^{2}}{2} v, 
    \label{eq:3}\tag{3}
\\]
$Z$ behaves like the Gaussian with the variance $v$.
The entropy method and transportation method are methods that control $\psi$ through the different but equivalent forms to Eq.$\,$($\ref{eq:3}$).

## Cramer-Chernoff method
In this post, we see why $  \mathbb{P}\\{ |Z - \mathop{\mathbb{E}} \left \lbrack Z \right \rbrack | \leq t\\} $ is bounded 
by  $\psi_{Z - \mathop{\mathbb{E}}\left \lbrack Z \right \rbrack }(\lambda)$
and 
where the condition $\psi_{Z - \mathop{\mathbb{E}}\left \lbrack Z \right \rbrack } \leq \frac{\lambda^{2}}{2} v $ comes from.
To this goal, letting $Y = Z - \mathop{\mathbb{E}}\left \lbrack Z \right \rbrack $, we begin by 
\\[
    \mathbb{P}\\{ Y \geq t\\}
    = 
    \mathbb{P}\\{ \exp(Y) \geq \exp(t)\\}
    =
    \mathbb{P}\\{ \exp(\lambda Y) \geq \exp(\lambda t)\\}
    \leq
    \frac
    {\mathop{\mathbb{E}}\left \lbrack \exp(\lambda Y) \right \rbrack}
    {\exp(\lambda t)},
\\]
where the last term is from Markov's inequality.
The logarithm of the moment generating function comes from the above.
Using the definition of Eq.$\,$($\ref{eq:2}$) and introducing
\\[
    \psi_{Y}^{\*}(t) 
    =
    \sup_{\lambda \geq 0} (\lambda t - \psi_{Y}(\lambda)),
\\]
we obtain Chernoff inequality
\\[
    \mathbb{P}\\{ Y \geq t\\}
    \leq
    \exp(- \psi_{Y}^{\*}(t)).
    \label{eq:4}\tag{4}
\\]

## Chernoff inequality for Gaussian random variable
We want to find $\lambda$ that minimizes the upper bound of Eq.$\,$($\ref{eq:4}$), $\exp(- \psi_{Y}^{\*}(\lambda))$,
and we consider its concrete form for the Gaussian random variable.
Let $Y$ be a centered Gaussian random variable with variance $v$ and thus a sample from $\mathcal{N}(0, v)$.
Note that 
it has not lost its generality compared to thinking $Z$ sampled from $\mathcal{N}(\mu, v)$
becuase of the relationship 
$Y = Z - \mathop{\mathbb{E}} \left \lbrack Z \right \rbrack$.
From the fact that 
the logarithm of the moment generating function for the Gaussian random variable is
\\[
    \psi_{Y} = \frac{\lambda^{2}}{2} v,
\\]
$\psi_{Y}^{\*}(t)$ can be written as 
\\[
    \psi_{Y}^{\*}(t) = \lambda t - \frac{\lambda^{2}}{2} v.
\\]
We want to know $\lambda$ that maxmizes $\psi_{Y}^{\*}(t)$, and it is obtained as 
\\[
    \frac{\partial }{\partial  \lambda} \psi_{Y}^{\*}(t) = t - \lambda v = 0
    \Leftrightarrow
    \lambda = \frac{t}{v}.
\\]
Thus we have 
$
    \psi_{Y}^{\*}(t) = \frac{t^{2}}{2 v},
$
and Chernoff inequality for the Gaussian random variable is
\\[
    \mathbb{P}\\{ Y \geq t\\}
    \leq
    \exp( - t^{2} / {2 v}).
\\]
\\[
\\]

---
layout: post
title: "Entropy Tensorization"
---

Entropy tensorization, or the sub-additivity property of the entropy, is one of the essencial components to derive binary log-sobolev inequality and Gaussain log-sobolev inequality.
Entropy tensorization is derived from the relationship between entropy and 
relative entropy through the tilting measure we saw in 
[this post]({{ site.baseurl }}{% post_url 2022-09-16-tilting-measure-and-relative-entropy%}) and
Han's inequality for relative entropy
that post, and from Han's inequality for KL divergence
we saw in
[this post]({{ site.baseurl }}{% post_url 2022-09-20-hans-inequality-for-relative-entropy%}).

## Chain rule for KL divergence
As a preparation for the following derivation, we first see chain rule for KL divergence.
Consider two distributions $P(X,Y)$ and $Q(X,Y)$
for a pair of variables $X$ and $Y$.
Since $p(x,y) = p(x)p(y|x)$,
From Bayes rule, we know
\[
    D(Q \parallel P) = \Sigma_{x,y} q(x) q(y|x) \log \left ( \frac{q(x)}{p(x)} \cdot \frac{q(y|x)}{p(y|x)} \right ).
\]
Thus, the LHS becomes
$
    D(Q \parallel P) = D  \left ( Q(X) \parallel P(X) \right ) +  \mathbb{E}_{y \sim P} \left \lbrack  D((P(X|Y = y) \parallel Q(X|Y = y)) \right \rbrack
$, and we have
\[
    D(Q \parallel P) = D(Q(X) \parallel P(X)) + D(P(X|Y) \parallel Q(X|Y)).
\]

## Entropy tensorization
As in [this post]({{ site.baseurl }}{% post_url 2022-09-16-tilting-measure-and-relative-entropy%})
we define $\Phi = x \log x$ for $x>0$, $\Phi(0)=0$, and
\[
    \text{Ent}(X) := \mathbb{E} \Phi(X) - \Phi(\mathbb{E}X).
\]
Let $X_{1}, \ldots, X_{n}$ be independent positive random variables
and $Z = f(X_{1}, \ldots, X_{n})$.
The notation $\mathop{\mathbb{E}}^{i}$ denotes expectation w.r.t. the variable $X_{i}$ only,
that is, conditional expectation conditioned on $X^{i} = X_{1}, \ldots, X_{i-1}, X_{i+1}, \ldots, X_{n}$.
We also introduce the notation 
\[
    \text{Ent}\^{i}(X) := \mathbb{E}^{i} \Phi(X) - \Phi(\mathbb{E}^{i} X).
\]
Then, entropy tensorization states
\[
    \text{Ent}(X) \leq \Sigma_{i=1}^{n} \mathbb{E} \left \lbrack \text{Ent}\^{i}(X) \right \rbrack .
\]

#### Proof
Let $P$ be the probability distribution of $X = (X_{1}, \ldots, X_{n})$.
We consider a tilting measure $Q = P_{Z}$, i.e., $Z = \frac{dQ}{dP}$.
If $\mathbb{E}_{P} Z = 1$,
then we know $\text{Ent}(Z) = \mathbb{E}_{P} [Z \log Z] = D(Q \parallel P)$. 

We start by Han's Inequality for KL divergence,
\[
    D(Q \parallel P) \leq \Sigma_{i=1}^{n} (D(Q \parallel P) - \color{blue}{D(Q^{i} \parallel P^{i})}).
    \label{eq:1}\tag{1}
\]
Then, splitting $X$ into $X_{i}$ and $X^{i}$ and applying chain rule for KL divergence, we have
\[
    D(Q \parallel P) = \color{blue}{D(Q(X^{i}) \parallel P(X^{i}))} + D(Q(X_{i}|X^{i}) \parallel P(X_{i}|X^{i})).
\]
Substituting this into the RHS of Eq.$\,$($\ref{eq:1}$), Eq.$\,$($\ref{eq:1}$) becomes
\[
   D(Q \parallel P) \leq \Sigma_{i=1}^{n} D(Q(X_{i}|X^{i}) \parallel P(X_{i}|X^{i})). 
\]
\[
\]
\[
\]
\[
\]

---
layout: post
title: "Han's Inequality and Entropy Tensorization"
---

Entropy tensorization, or the sub-additivity property of the entropy, is one of the ingradients to derive binary log-sobolev inequality and Gaussain log-sobolev inequality.
In this post, we first see Han's inequality for KL divergence, from which Entropy tensorization is directly derived. 

## Han's Inequality
Han's Inequality states
\[
    H(X_{1}, \ldots, X_{n}) \leq \frac{1}{n-1} \Sigma_{i=1}^{n} H(X_{1}, \ldots, X_{i-1}, X_{i+1}, \ldots, X_{n}),
    \label{eq:1}\tag{1}
\]
where $H(X)$ is the Shannon entoropy, $-\Sigma P \log P$, $P = p(x)$.
Note that $X_{1}, \ldots, X_{n}$ do not need to be independent.

#### Proof
We use two basic property of the conditional entropy:
\[
    H(X,Y) = H(X|Y) + H(Y)
\]
and
\[
    H(X) \leq H(X|Y).
\]
From the first one, we obtain the chain rule of entropy:
\[
    H(X_{1}, \ldots, X_{n}) = H(X_{1}) + H(X_{2}|X_{1}) + H(X_{3}| X_{1},X_{2}) + \cdots + H(X_{n}| H(X_{1}, \ldots, X_{n-1}).
\]

Using the first one yields
\[
    H(X_{1}, \ldots, X_{n}) = H(X_{1}, \ldots, X_{i-1}, X_{i+1}, \ldots, X_{n}) + H(X_{i} | X_{1}, \ldots, X_{i-1}, X_{i+1}, \ldots, X_{n}),
\]
and applying the second inequality to the second term in the RHS tells us that
\[
    H(X_{i} | X_{1}, \ldots, X_{i-1}, X_{i+1}, \ldots, X_{n}) \leq H(X_{i} | X_{1}, \ldots, X_{i-1}).
\]
Summing $n$ inequalities for different $i$, we have 
\[
    n H(X_{1}, \ldots, X_{n}) \leq \Sigma_{i=1}^{n} H(X_{1}, \ldots, X_{i-1}, X_{i+1}, \ldots, X_{n}) + H(X_{1}) + H(X_{2}|X_{1}) + H(X_{3}| X_{1},X_{2}) + \cdots + H(X_{n}| H(X_{1}, \ldots, X_{n-1}).
\]
Applying the chain rule of entropy,
\[
    n H(X_{1}, \ldots, X_{n}) \leq \Sigma_{i=1}^{n} H(X_{1}, \ldots, X_{i-1}, X_{i+1}, \ldots, X_{n}) + H(X_{1}, \ldots, X_{n}),
\] 
and thus it becomes the statement,
\[
    (n-1) H(X_{1}, \ldots, X_{n}) \leq \Sigma_{i=1}^{n} H(X_{1}, \ldots, X_{i-1}, X_{i+1}, \ldots, X_{n}).
\] 
The source of the inequality is the relationship $H(X) \leq H(X|Y)$, meaning that conditioning decreases entropy.

## Han's inequality for KL divergence
Symbols with superscript $i$ such as $x^{i}$ and $P^{i}$ denote $n-1$ dimensional vectors or distributions marginalized over the $i$-th element, meaning that the $i$-th element is excluded.
Han's inequality for KL divergence states
\[
    D(Q \parallel P) \geq \frac{1}{n-1} \Sigma_{i=1}^{n} D(Q^{i} \parallel P^{i})
    \label{eq:2}\tag{2}
\]
or equivalently,
\[
    D(Q \parallel P) \leq \Sigma_{i=1}^{n} (D(Q \parallel P) - D(Q^{i} \parallel P^{i})).
    \label{eq:3}\tag{3}
\]

#### Proof
Denoting $\Sigma_{x \in \mathcal{X}^{n}}$ by $\Sigma$
and $- \Sigma Q \log Q$ by $H_{Q}(X)$,
we have
\[
    D(Q \parallel P) = \color{green}{- \Sigma Q \log P} - H_{Q}(X)
\]
and
\[
    D(Q^{i} \parallel P^{i}) = \color{blue}{- \Sigma_{x^{i} \in \mathcal{X}^{n-1}} Q^{i} \log P^{i}} - H_{Q^{i}}(X^{i})
\]
where 
$H_{Q^{i}}(X^{i}) = - \Sigma_{x^{i} \in \mathcal{X}^{n-1}} Q^{i} \log Q^{i}$.
Since Han's inequality (Eq.$\,$($\ref{eq:1}$)) tells us
\[
    - H_{Q}(X) \leq - \frac{1}{n-1} \Sigma_{i=1}^{n} H_{Q^{i}}(X^{i}),
\]
if 
\[
    \color{green}{- \Sigma Q \log P} \geq \frac{1}{n-1} \Sigma_{i=1}^{n} (\color{blue}{- \Sigma_{x^{i} \in \mathcal{X}^{n-1}} Q^{i} \log P^{i}}),
\]
we can say that (Eq.$\,$($\ref{eq:2}$)) holds.
Indeed, it turns out that equality holds in the above, as we will see.
Using the product property of $P$, i.e., $P = P^{i}P_{i}$ and $P=\prod_{i=1}^{n}P_{i}$, we have
\[
    \color{green}{- \Sigma Q \log P} = - \Sigma Q(\log P^{i} + \log P_{i}) = - \frac{1}{n} \Sigma_{i=1}^{n} \Sigma Q(\log P^{i} + \log P_{i}).
\]
Since
$
    \Sigma_{i=1}^{n} \log P_{i} = \log \prod_{i=1}^{n}P_{i} =  \log P,
$
we have
\[
    \color{green}{- \Sigma Q \log P} = - \frac{1}{n} \Sigma_{i=1}^{n} \Sigma Q(\log P^{i} 
    - \frac{1}{n} \Sigma Q \log P).
\]
Rearranging, we have
\[
    \color{green}{- \Sigma Q \log P} = - \frac{1}{n-1}\Sigma_{i=1}^{n} \Sigma Q \log P^{i}.
\]
Since $\Sigma Q = \Sigma_{x^{i} \in \mathcal{X}^{n-1}} \Sigma_{x_{i} \in \mathcal{X}^{1}} Q^{i} Q_{i}$,
we have
\[
     \color{green}{- \Sigma Q \log P} = \frac{1}{n-1} \Sigma_{i=1}^{n} (\color{blue}{- \Sigma_{x^{i} \in \mathcal{X}^{n-1}}     Q^{i} \log P^{i}}).
\]
    
## Entropy tensorization
As in [this post]({{ site.baseurl }}{% post_url 2022-09-16-tilting-measure-and-relative-entropy%})
we define $\Phi = x \log x$ for $x>0$, $\Phi(0)=0$, and
\[
    \text{Ent}(X) := \mathbb{E} \Phi(Z) - \Phi(\mathbb{E}Z).
\]
Let $X_{1}, \ldots, X_{n}$ be independent positive random variables
and $Z = f(X_{1}, \ldots, X_{n}$.
The notation $\mathop{\mathbb{E}}^{i}$ denotes expectation w.r.t. the variable $X_{i}$ only,
that is, conditional expectation conditioned on $X^{i} = {X_{1}, \ldots, X_{i-1}, X_{i+1}, \ldots, X_{n}}$.
We also introduce the notation 
$
    \text{Ent}_{i}(X) := \mathbb{E}^{i} \Phi(Z) - \Phi(\mathbb{E}^{i} Z).
$
Then, entropy tensorization states
\[
    \text{Ent}(Z) \leq \mathbb{E} \left \lbrack \Sigma_{i=1}^{n} \text{Ent}_{i}(Z) \right \rbrack .
\]

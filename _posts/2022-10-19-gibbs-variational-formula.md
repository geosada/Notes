---
layout: post
title: "Gibbs Variational Formula and Variational Formula for Divergence"
---
Other than the entropy method, the transportation method is another method that controls 
the logarithm of the moment generating function.
In this post we introduce Gibbs variational formula, which is used to derive the transportation lemma.

## Gibbs Variational Formula
Let $P$ be a probability measure on $\Omega$ and $Z: \Omega \mapsto \mathbb{R}$ be any random variable.
Then, Gibbs variational formula states
\[
    \log \mathbb{E}\_{P} e^{ Z - \mathbb{E}\_{P} \left \lbrack Z \right \rbrack }
    =
    \sup_{Q \lt \lt P}
    \left(
        \mathbb{E}\_{Q} \left \lbrack Z \right \rbrack 
        -
        \mathbb{E}\_{P} \left \lbrack Z \right \rbrack
        -
        D(Q \parallel P)
    \right),
    \label{eq:1}\tag{1}
\]
which says that
complicated $\log \mathbb{E}\_{P} e^{ Z - \mathbb{E}\_{P} \left \lbrack Z \right \rbrack }$
can be decomposed into simple $\mathbb{E}\_{Q}$ and $\mathbb{E}\_{P}$.
The transformation from $P$ to $Q$ is the *tilting* introduced 
in [this post]({{ site.baseurl }}{% post_url 2022-09-16-tilting-measure-and-relative-entropy %}),
and its variations are innumerable,
but interstingly, when $Q$ is the exponential tilting of $P$, i.e.,
$
    Q =
    P_{ 
        \frac
        {e^{Z}} 
        {\mathbb{E} \left \lbrack e^{Z} \right \rbrack }
    }   
$,
the supremum is achieved.

#### Proof
Let 
$
    R =
    P_{ 
        \frac
        {e^{Z}} 
        {\mathbb{E}\_{p} \left \lbrack e^{Z} \right \rbrack }
    }   
$ and thus
$\frac{dR}{dP} = \frac {e^{Z}}  {\mathbb{E}\_{p} \left \lbrack e^{Z} \right \rbrack }  $,
then we have
\[
    D(Q \parallel R)
    = 
    \mathbb{E}\_{Q} \left \lbrack \log \frac{dQ}{dR} \right \rbrack
    =
    \mathbb{E}\_{Q} \left \lbrack \log \frac{dQ}{dP} \right \rbrack
    -
    \mathbb{E}\_{Q} \left \lbrack \log \frac{dR}{dP} \right \rbrack
    =
    \mathbb{E}\_{Q} \left \lbrack \log \frac{dQ}{dP} \right \rbrack
    -
    \mathbb{E}\_{Q} \left \lbrack \log \frac {e^{Z}}  {\mathbb{E}\_{p} \left \lbrack e^{Z} \right \rbrack } \right \rbrack
\]
The LHS is decomposed further as
\[
    =
    \mathbb{E}\_{Q} \left \lbrack \log \frac{dQ}{dP} \right \rbrack
    -
    \mathbb{E}\_{Q} \left \lbrack \log e^{Z} \right \rbrack
    +
    \mathbb{E}\_{Q} \left \lbrack \log {\mathbb{E}\_{p} \left \lbrack e^{Z} \right \rbrack } \right \rbrack
    =
    D(Q \parallel P)
    -
    \mathbb{E}\_{Q} \left \lbrack Z \right \rbrack
    +
    \log {\mathbb{E}\_{p} \left \lbrack e^{Z} \right \rbrack }.
\]
Since $D(Q \parallel R) \geq 0$, we obtain
\[
    \log {\mathbb{E}\_{p} \left \lbrack e^{Z} \right \rbrack }
    \geq
    \mathbb{E}\_{Q}  \left \lbrack Z \right \rbrack 
    -
    D(Q \parallel P),
    \label{eq:2}\tag{2}
\]
and replacing $Z$ with $Z - \mathbb{E}\_{P} \left \lbrack Z \right \rbrack $ yields 
\[
    \log \mathbb{E}\_{P} e^{ Z - \mathbb{E}\_{P} \left \lbrack Z \right \rbrack }
    \geq
        \mathbb{E}\_{Q} \left \lbrack Z \right \rbrack 
        -
        \mathbb{E}\_{P} \left \lbrack Z \right \rbrack
        -
        D(Q \parallel P).
\]
Thus, taking $\sup_{Q \lt \lt P}$ results in the statement.
The equality holds when
$
    Q =  
    R =
    P_{ 
        \frac
        {e^{Z}} 
        {\mathbb{E} \left \lbrack e^{Z} \right \rbrack }
    }   
$, because in that case $D(Q \parallel R) =0$.

## Variational formula for KL Divergence
As Eq.$\,$($\ref{eq:2}$) says 
$
    D(Q \parallel P)
    \geq
    \mathbb{E}\_{Q}  \left \lbrack Z \right \rbrack 
    -
    \log {\mathbb{E}\_{p} \left \lbrack e^{Z} \right \rbrack }
$
for every $Q \lt \lt P$ and every $Z$ such that $\mathbb{E}\_{p} \left \lbrack e^{Z} \right \rbrack \lt \infty$,
it indicates
\[
    D(Q \parallel P)
    =
    \sup_{Z: \mathbb{E}\_{p} \left \lbrack e^{Z} \right \rbrack \lt \infty}
    \mathbb{E}\_{Q}  \left \lbrack Z \right \rbrack 
    -
    \log {\mathbb{E}\_{p} \left \lbrack e^{Z} \right \rbrack }.
    \label{eq:3}\tag{3}
\]
Letting $Z = \log \frac{dQ}{dP}$, we have
\[
    \mathbb{E}\_{Q}  \left \lbrack Z \right \rbrack 
    -
    \log {\mathbb{E}\_{p} \left \lbrack e^{Z} \right \rbrack }
    =
    \mathbb{E}\_{Q}  \left \lbrack \log \frac{dQ}{dP} \right \rbrack
    -
    \log \mathbb{E}\_{p} \left \lbrack \frac{dQ}{dP} \right \rbrack ,
\]
and because of
\[
    {\mathbb{E}\_{p} \left \lbrack \frac{dQ}{dP} \right \rbrack } = 1
\]
from the definition of the tilting, the second term is $0$.
Since 
\[
    \mathbb{E}\_{Q}  \left \lbrack \log \frac{dQ}{dP} \right \rbrack
    =
    D(Q \parallel P),
\]
it indicates that the equality of Eq.$\,$($\ref{eq:3}$) holds with $Z = \log \frac{dQ}{dP}$.

## Another Variational formula for KL Divergence
 
#### Jensen’s gap variational formula
For a convex function  $f: I \mapsto \mathbb{R}$,
the following is know as Jensen's gap variational formula:
\[
    \mathbb{E}  \left \lbrack f(x) \right \rbrack - f \left ( \mathbb{E}  \left \lbrack x \right \rbrack \right)
    =
    \min_{a \in I} \mathbb{E}  \left \lbrack f(x) - f(a) - f'(a) (x-a) \right \rbrack.
\]
The equality holds when $a = \mathbb{E}  \left \lbrack x \right \rbrack $, 
because the last term becomes
$ f'(a) (\mathbb{E}  \left \lbrack x \right \rbrack - \mathbb{E}  \left \lbrack x \right \rbrack) = 0$.

#### Application to variance
Consider $f(x) = x^{2}$, then we have
\[
    \mathbb{E}  \left \lbrack x^{2} \right \rbrack -  \mathbb{E}  \left \lbrack x \right \rbrack ^{2}
    = \text{Var}(x)
    = \mathbb{E}  \left \lbrack (x - \mathbb{E} \left \lbrack x \right \rbrack) ^{2}\right \rbrack.
\]
This can be expressed as
\[
    = \min_{a} \mathbb{E}  \left \lbrack x^{2} - a^{2} -2a (x-a) \right \rbrack
    = \min_{a} \mathbb{E}  \left \lbrack (x - a) ^{2}\right \rbrack,
\]
meaning that the expected value minimizes the MSE.

#### Application to entropy
Consider $f(x) = x \log x$ for $x \in [0, \infty)$, then we have
\[
    \mathbb{E} \left \lbrack  x \log x \right \rbrack
    - \mathbb{E} \left \lbrack x \right \rbrack \log \mathbb{E} \left \lbrack x \right \rbrack
    =
    \text{Ent}(x)
    =
    \min_{a \gt 0} \mathbb{E} \left \lbrack x \log x - a \log a - (1 + \log a) (x -a) \right \rbrack
    =
    \min_{a \gt 0} \mathbb{E} \left \lbrack x \log \frac{x}{a} - (x -a) \right \rbrack
\]
When $x = \frac{dQ}{dP}$,
\[
    \text{Ent}(x) = D(Q \parallel P) = 
    \min_{a \gt 0} \mathbb{E}\_{p} \left \lbrack \frac{dQ}{dP} \log \frac{dQ}{dP} - \log a - (\frac{dQ}{dP} -a) \right \rbrack,
\]
which is another variational formula for KL divergence.


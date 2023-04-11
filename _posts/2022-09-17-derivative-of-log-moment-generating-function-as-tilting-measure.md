---
layout: post
title: "Derivative of Log Moment Generating Function as Tilting Measure"
---
The first and second derivertive of
log moment generating function for $Z$, $\psi(\lambda)$,
can be expressed 
with exponential tilting by
$
    \frac
    {e^{\lambda Z}} 
    {\mathop{\mathbb{E}} \left \lbrack e^{\lambda Z} \right \rbrack }
$
introduced in [this post]({{ site.baseurl }}{% post_url 2022-09-16-tilting-measure-and-relative-entropy %}),
and from this the convexity of $\psi(\lambda)$ is shown.
Using the fact we see in this post, Hoeffding's inequality is immediately derived, as we will see in
[the later post]({{ site.baseurl }}{% post_url 2022-09-18-herbst-argument-and-entropy-method%}).

As before,
$\psi(\lambda)$ denotes
$
    \log \mathbb{E} e^{ \lambda (Z - \mathbb{E} Z )}
$
and by exponential tilting,
the original measure $P$ is tilted into 
$
    Q = P_{ 
    \frac
    {e^{\lambda Z}} 
    {\mathbb{E} \left \lbrack e^{\lambda Z} \right \rbrack }
    }.  
$
Then, we have
\[
    \psi'(\lambda) = \mathbb{E}\_{Q} \left \lbrack Z - \mathbb{E}Z \right \rbrack,
\]
\[
    \psi'\'(\lambda) = \text{Var}\_{Q} \left \lbrack Z \right \rbrack,
\]
and
\[
    \psi(\lambda) = \psi''(\lambda) = 0, 
    \ \ 
    \psi'\'(0) = \text{Var} \left \lbrack Z \right \rbrack,
\]
where 
$\mathbb{E}$ and $\text{Var}$ is the moment over $P$ as
$\mathbb{E} = \mathbb{E}\_{P}$ and $\text{Var} = \text{Var}\_{P}$.

#### First derivertive
As  $
\psi(\lambda) = \log \mathbb{E} e^{ \lambda Z  } - \log \mathbb{E} e^{ \lambda \mathbb{E} Z } 
= \log \mathbb{E} e^{ \lambda Z  } - \lambda \mathbb{E} Z$,
we know
\[
    \psi(0) = 0.
\]
The well known property of $\psi(\lambda)$ is that it is differentiable any number of times.
The first derivertive is
\[
    \psi'(\lambda)
    =
    \frac
    { \mathbb{E} \left \lbrack Z e^{\lambda Z} \right \rbrack }
    { \mathbb{E} \left \lbrack e^{\lambda Z} \right \rbrack }
    -
    \mathbb{E} Z
    =
    \mathbb{E}\_{Q} Z - \mathbb{E} Z
    =
    \mathbb{E}\_{Q} \left \lbrack Z - \mathbb{E} Z \right \rbrack.
\]
From the fact that $Q = P$ when $\lambda = 0$, we know
\[
    \psi'(0) = 0.
\]

#### Second derivertive
Next, since $\mathbb{E} Z$ is constant w.r.t. $\lambda$, we have
\[
    \psi'\'(\lambda)
    =
    \frac{d}{d \lambda}
    \left (
    \frac
    { \mathbb{E} \left \lbrack Z e^{\lambda Z} \right \rbrack }
    { \mathbb{E} \left \lbrack e^{\lambda Z} \right \rbrack }
    \right ).
\]
As $ \left ( \frac{f}{g} \right ) = \frac{1}{g^{2}} \left ( f'g - fg' \right ) $,
the above is 
\[
    = \frac{1}{  \left ( \mathbb{E} \left \lbrack e^{\lambda Z} \right \rbrack \right )^{2}}
    \left (
    \frac{d}{d \lambda}
        \left (
            \mathbb{E} \left \lbrack Z e^{\lambda Z} \right \rbrack 
        \right )
        \mathbb{E} \left \lbrack e^{\lambda Z} \right \rbrack 
    -
    \mathbb{E} \left \lbrack Z e^{\lambda Z} \right \rbrack
    \frac{d}{d \lambda}
        \left (
            \mathbb{E} \left \lbrack e^{\lambda Z} \right \rbrack 
        \right )
    \right )
\]
\[
    = \frac{1}{  \left ( \mathbb{E} \left \lbrack e^{\lambda Z} \right \rbrack \right )^{2}}
    \left (
        \mathbb{E} \left \lbrack Z^{2} e^{\lambda Z} \right \rbrack \mathbb{E} \left \lbrack e^{\lambda Z} \right \rbrack
        -
        \mathbb{E} \left \lbrack Z e^{\lambda Z} \right \rbrack \mathbb{E} \left \lbrack Z e^{\lambda Z} \right \rbrack
    \right ).
\]
By transforming the measure from $P$ to $Q$, the above is
\[
    =\mathbb{E}\_{Q} Z^{2} - \left ( \mathbb{E}\_{Q} Z \right )^{2}
    = \text{Var}\_{Q} \left \lbrack Z \right \rbrack
    \geq 0.
\]
As the second derivatibe is grater than $0$, it shows that $\psi(\lambda)$ is convex.

![$psi$]({{site.baseurl}}/img/Chernoff/fig_psi_lambda.png){: .centered width="350" }

Lastly, from the fact that $Q = P$ when $\lambda = 0$, we know
\[
    \psi'\'(0) = \text{Var}(Z).
\]

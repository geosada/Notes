---
layout: post
title: "Herbst’s Argument and Entropy Method"
---
With
the notion of the *tilting* and its connection to the relative entropy
introduced in [this post]({{ site.baseurl }}{% post_url 2022-09-16-tilting-measure-and-relative-entropy %}),
we will see the entropy method in this post.
As described in [this post]({{ site.baseurl }}{% post_url 2022-09-01-chernoff-bounds%}),
when a random raviable $Z$ satisfies
\\[
    \psi_{Z - \mathop{\mathbb{E}}\left \lbrack Z \right \rbrack }(\lambda)
    \leq
    \frac{ \lambda^{2}}{2} v
    \ \ \forall \lambda \gt 0,
    \label{eq:1}\tag{1}
\\]
$Z$ is sub-Gaussian and concentrated around its mean as
\\[
    \mathbb{P}\\{ |Z - \mathop{\mathbb{E}} \left \lbrack Z \right \rbrack | \leq t\\}
    \leq
    \exp( -\frac{t^{2}}{2 v})
    \ \ \forall t \gt 0.
    \label{eq:2}\tag{2}
\\]
where
$
    \psi_{Z - \mathop{\mathbb{E}}\left \lbrack Z \right \rbrack }(\lambda)
    =
    \log \mathop{\mathbb{E}} \exp \left (\lambda (Z - \mathop{\mathbb{E}} \left \lbrack Z \right \rbrack) \right )
$
is the logarithm of the moment generating function.
Instead of directly bounding $\psi_{Z - \mathop{\mathbb{E}}\left \lbrack Z \right \rbrack }(\lambda)$,
entropy method bounds 
$
    \frac{1}{\lambda^{2}}
    \frac
    {\text{Ent}(e^{\lambda Z})}
    {\mathop{\mathbb{E}} \left \lbrack e^{\lambda Z} \right \rbrack },
$
which is equivalent to 
the divergence caused by the tilting of 
$
    \frac{e^{\lambda Z}}{\mathbb{E} e^{\lambda Z}},
$
$
    \frac{1}{\lambda^{2}}
    D(P_{\frac{e^{\lambda Z}}{\mathbb{E} e^{\lambda Z}}} \parallel P)
$.
What brings this connection is Herbst's argument.

## Herbst’s argument 
To show 
$
    \psi(\lambda)
    \leq
    \frac{ \lambda^{2}}{2} v
    \ \ \forall \lambda \gt 0
$
is equivalent to showing
\\[
   \Leftrightarrow
   \frac{\psi(\lambda)}{\lambda}
   \leq
    \frac{ \lambda}{2} v
   \Leftrightarrow
   \color{green}{
   \frac{d}{d \lambda}
   \left (
   \frac{\psi(\lambda)}{\lambda}
   \right )
   }
   \leq
   \frac{v}{2},
\\]
and we focus on 
$
   \frac{d}{d \lambda}
   \left (
   \frac{\psi(\lambda)}{\lambda}
   \right ).
$
\\[
   \color{green}{
   \frac{d}{d \lambda}
   \left (
   \frac{\psi(\lambda)}{\lambda}
   \right )
   }
   =
   \frac{d}{d \lambda}
   \left (
   \frac{1}{\lambda}
   \log \mathbb{E} e^{ \lambda (Z - \mathop{\mathbb{E}} \left \lbrack Z \right \rbrack) }
   \right )
   =
   \frac{d}{d \lambda}
   \left (
   \frac{1}{\lambda}
   \left (
   \log \mathbb{E} e^{ \lambda Z} - \lambda \mathbb{E}\left \lbrack Z \right \rbrack 
   \right )
   \right ).
\\]
Since $𝔼[Z]$ is constant, the above is 
\\[
   \frac{d}{d \lambda}
   \left (
   \frac{1}{\lambda}
   \left (
   \log \mathbb{E} e^{ \lambda Z}
   \right )
   \right )
   =
   \frac{1}{\lambda^{2}}
   \left (
   \frac{\mathbb{E} \left \lbrack Z e^{\lambda Z} \right \rbrack  }
   {\mathbb{E} \left \lbrack e^{\lambda Z} \right \rbrack }
   -
   \log \mathbb{E} e^{ \lambda Z} 
   \cdot 1
   \right )
   = 
   \frac{1}{\lambda^{2} \mathbb{E} \left \lbrack e^{\lambda Z} \right \rbrack }
   \left (
   \color{blue}{
   \lambda \mathbb{E} \left \lbrack Z e^{\lambda Z} \right \rbrack 
   }
   -
   \mathbb{E} \left \lbrack e^{\lambda Z} \right \rbrack \cdot \log \mathbb{E} e^{ \lambda Z}
   \right ).
\\]
Note that
\\[
   \color{blue}{
   \lambda \mathbb{E} \left \lbrack Z e^{\lambda Z} \right \rbrack 
   }
   =
   \mathbb{E} \left \lbrack \lambda Z e^{\lambda Z} \right \rbrack
   =
   \mathbb{E} \left \lbrack e^{\lambda Z} \lambda Z \right \rbrack
   =
   \mathbb{E} \left \lbrack e^{\lambda Z} \log e^{\lambda Z} \right \rbrack
   .
\\]
Recall
$
    \text{Ent}(X) := \mathbb{E} \left \lbrack X \log X \right \rbrack
    -
    \mathbb{E} \left \lbrack X \right \rbrack \log \mathbb{E} \left \lbrack X \right \rbrack
$,
we obtain
\\[
    \color{green}{
    \frac{d}{d \lambda}
    \left (
    \frac{\psi(\lambda)}{\lambda}
    \right )
    }
    =
    \frac
    {\text{Ent} (e^{\lambda Z})}
    { \lambda^{2} \mathbb{E} \left \lbrack e^{\lambda Z} \right \rbrack }
    = 
    \frac{1}{\lambda^{2}}
    D(Q \parallel P),
    \ \ 
    Q = P_{
    \frac
    {e^{\lambda Z}}
    {\mathop{\mathbb{E}} \left \lbrack e^{\lambda Z} \right \rbrack }
    }.
\\]
Thus, Herbst’s argument says that if the exponential tilting based on $Z$ is less than 
$
    \frac{\lambda^{2}}{2}v
$
for all $\lambda \gt 0$,
then Eq.$\,$($\ref{eq:1}$) and thus Eq.$\,$($\ref{eq:2}$) holds for such $Z$,
meaning $Z$ is sub-Gaussian.

## Deriving Hoeffding's inequality by Herbst’s argument
Hoeffding's lemma states that
if $X \in [a,b]$, then $X$ is $\frac{(b-a)^{2}}{4}$-sub-Gaussian.

#### Preparation
We know that $X \in [a,b]$ satisfies
$
|X - \mu| \leq \frac{b-a}{2}
$
as
![Hoeffding inequality]({{site.baseurl}}/img/Chernoff/fig_hoeffding.png){: .centered width="350" }
where the length of yellow regions is $\frac{b-a}{2}$.
Letting $Y = X - \mu$, since $\text{Var}$ is invariant to the transition,
\\[
    \text{Var}(X) = \text{Var}(Y) 
    = 
    \mathbb{E} \left \lbrack Y^{2} \right \rbrack - (\mathbb{E} Y)^{2}.
\\]
As $\mathbb{E} Y = 0$, the above is
\\[
    = \mathbb{E} \left \lbrack Y^{2} \right \rbrack
    \leq
    \mathbb{E} \left \lbrack 
        \left (
            \frac{b-a}{2}
        \right )
    \right \rbrack^{2}
    =
    \left (
        \frac{b-a}{2}
    \right )
    ^{2},
\\]
and thus we have
\\[
    \text{Var}(X) \leq \frac{(b-a)^{2}}{4}.
    \label{eq:3}\tag{3}
\\]

#### Proof 1
The first way is the one uses the fact we saw 
in [this post]({{ site.baseurl }}{% post_url 2022-09-17-derivative-of-log-moment-generating-function-as-tilting-measure %}),
$ \psi_{X}'\'(\lambda) = \text{Var}\_Q(X) $.
Eq.$\,$($\ref{eq:3}$) holds no matter what the measure is
and thus in other words, it is valid no matter how the measure $P$ is tilted.
So we have
\\[
    \psi_{X}'\'(\lambda) = \text{Var}\_Q(X) = \text{Var}(X) \leq \frac{(b-a)^{2}}{4},
\\]
 indicating 
\\[
    \psi_{X}(\lambda )
    \leq
    \frac{\lambda^{2}}{2} \frac{(b-a)^{2}}{4},
\\]
and thus we obtain the states of Hoeffding's lemma.

#### Proof 2
We show alternative proof using Herbst’s argument.
Herbst’s argument says
\\[
    \frac
    {\text{Ent} (e^{\lambda X})}
    { \mathbb{E} \left \lbrack e^{\lambda X} \right \rbrack }
    =
    \lambda^{2}
    \color{green}{
    \frac{d}{d \lambda}
    \left (
        \frac{\psi(\lambda)}{\lambda}
    \right )
    }
    =
    \lambda^{2}
    \left (
        \psi'(\lambda) \frac{1}{\lambda}
        -
        \psi(\lambda) \frac{1}{\lambda^{2}}
    \right )
    =
    \lambda \psi'(\lambda) - \psi(\lambda),
\\]
and the RHS can be written as
\\[
    \lambda \psi'(\lambda) - \psi(\lambda)
    =
    \int_{0}^{\lambda} \theta \ \psi'\'(\theta) d \theta
    =
    \int_{0}^{\lambda} \theta \ \text{Var}\_Q(X)  d \theta
\\]
Using Eq.$\,$($\ref{eq:3}$) like in the proof 1, the above is bounded as
\\[
    \leq 
    \int_{0}^{\lambda} \theta \ \frac{(b-a)^{2}}{4}  d \theta
    =
    \frac{(b-a)^{2}}{4} \frac{\theta^{2}}{2} \bigg\rvert_{0}^{\lambda}
    = 
    \frac{\lambda^{2}}{2} \frac{(b-a)^{2}}{4}.
\\]
It indicates 
\\[
    \frac
    {\text{Ent} (e^{\lambda X})}
    { \mathbb{E} \left \lbrack e^{\lambda X} \right \rbrack }
    \leq
    \frac{\lambda^{2}}{2} \frac{(b-a)^{2}}{4},
\\]
which is equivalent to indicating 
\\[
    \psi_{X}(\lambda )
    \leq
    \frac{\lambda^{2}}{2} \frac{(b-a)^{2}}{4},
\\]
because of Herbst’s argument.
It shows $\frac{(b-a)^{2}}{4}$-sub-Gaussianity, and Hoeffding’s lemma has been proven.

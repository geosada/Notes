---
layout: post
title: "Hypercontractivity of Markov Kernel"
---
Hypercontractivity is the notion that connects to Gaussian logarithmic Sobolev inequality and Data processing inequality.
In this post we will see Hypercontractivity 
begining by describing contractivity as preparation.
This post is based on [the lecture](https://www.youtube.com/watch?v=YOT95NQztbk) by Himanshu Tyagi.

## $L_{p}$ Norm
For $1 \leq p$ and any function $f:\mathcal{X} \mapsto \mathbb{R}$, the $L_{p}$ norm of $f$ is 
\\[
    \lVert f \rVert_{p} = \left \( \int_{\mathcal{X}} |f(x)|^{p} dx \right \)^{\frac{1}{p}} = \mathop{\mathbb{E}} \left \lbrack f(x)^{p} \right \rbrack ^{\frac{1}{p}}.
\\]

## Markov Kernel
Consider two random variable $x$ and $y$ with joint distribution $p_{xy}$.
Letting $f: \mathcal{X} \mapsto \mathbb{R}$, we define $g: \mathcal{Y} \mapsto \mathbb{R}$ such that
\\[
	g(\mathscr{y}) := \mathop{\mathbb{E}} \left \lbrack f(x)|y= \mathscr{y} \right \rbrack,
\\]
where $\mathscr{y}$ is the realization of $y$.
The $\textit{channel}$ is denoted by $W$. 
The $W$ is the operator that converts $f$ to $g$ as $g=Wf$, and it corresponds to Markov kernel.
$W$ can be thought of as a transition matrix with an innumerable number of rows and columns.
The following figure illustrates how $W$ works.

![Narkov Kernel]({{site.baseurl}}/img/HC/fig_g_wf.png){: .centered width="450" }

It would be ideal if we could get $f(y)$ directly,
but what we can observe is a signal passsing through some noisy channel.
The noise in the channel is decided by $P_{x|y}$ and depends on $y$.
Thus, 
\\[
    g(\mathscr{y}) = W f(\mathscr{y}) = \mathop{\mathbb{E}}\_{x \sim p_{x|y= \mathscr{y}}} \left \lbrack f(x) \right \rbrack.
\\]
## Contraction for $L_{p}$ Norm
In the above setting, $W$ is contraction for $L_{p}$ norm, that is, for $1 \leq p$,
\\[
    \lVert Wf \rVert_{p} \leq \lVert f \rVert_{p} \label{eq:1}\tag{1}.
\\]
The proof begins with
\\[
    \lVert Wf \rVert_{p}^{p} = \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack \mathop{\mathbb{E}}\_{x \sim p_{x|y= \mathscr{y}}} \left \lbrack f(x)|y= \mathscr{y} \right \rbrack^{p} \right \rbrack.
\\]
By Jensen's inequality, the RHS is bounded as
\\[
    \leq
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack \mathop{\mathbb{E}}\_{x \sim p_{x|y= \mathscr{y}}} \left \lbrack f(x)^{\ p}|y= \mathscr{y} \right \rbrack \right \rbrack.
\\]
Because the inner conditional expectation is canceled by the outer expectation, the above becomes
\\[
    = \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack f(x)^{\ p} \rbrack \right \rbrack 
    =
    \lVert f \rVert_{p}^{p}.
\\]

## Hypercontractivity
While the contraction written as Eq.$\,$($\ref{eq:1}$) is the conversion between $p$-th norm and $p$-th norm, 
Hypercontractivity allows us to connect $p$-th norm to $q$-th norm where $q\lt p$.
A joint distribution of $x$ and $y$, $P_{xy}$, is $(p,q)$-hypercontractivity for $1 \lt q \leq p \lt \infty$, if
\\[
    \lVert Wf \rVert_{p} \leq \lVert f \rVert_{q} \label{eq:2}\tag{2}
    \ \  \forall f \in L_{q}(x).
\\]
Since $L_{p}$ norm is non-decrease function of $p$,
Eq.$\,$($\ref{eq:2}$) is stronger than Eq.$\,$($\ref{eq:1}$) when $q \lt p$.

## Two-function hypercontractivity
Eq.$\,$($\ref{eq:2}$) holds if and only if
\\[
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack f(x)g(y)\right \rbrack
    \leq
    \lVert f \rVert_{q} \lVert g \rVert_{p'}
    \ \  \forall g \in L_{p}(y), \label{eq:3}\tag{3}
\\]
where $p'$ is Hölder conjugate of  $p$, i.e., $p' = \frac{p}{p-1}$.

#### From Eq.$\,$($\ref{eq:2}$) to Eq.$\,$($\ref{eq:3}$)
We first see the proof of the direction from Eq.$\,$($\ref{eq:2}$) to Eq.$\,$($\ref{eq:3}$). 
As
\\[
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack f(x)g(y)\right \rbrack
    = 
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack W f(y)g(y)\right \rbrack,
\\]
by applying Hölder's inequality, the RHS is bounded as
\\[
    \leq \lVert f \rVert_{p} \lVert g \rVert_{p'}.
\\]
Since we are assuming Eq.$\,$($\ref{eq:2}$), the above is bounded as
\\[
    \leq \lVert f \rVert_{q} \lVert g \rVert_{p'},
\\]
saying that Eq.$\,$($\ref{eq:3}$) is true.

#### From Eq.$\,$($\ref{eq:3}$) to Eq.$\,$($\ref{eq:2}$)
Suppose Eq.$\,$($\ref{eq:3}$) holds any $g$
and consider $f \in L_{p}(x)$.
Then,
\\[
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack W f(y)^{p} \right \rbrack
    =
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack W f(y) \ W f(y)^{p-1} \right \rbrack
    =
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack \mathop{\mathbb{E}}\_{x \sim p_{x|y= \mathscr{y}}} \left \lbrack f(x) \right \rbrack  \ W f(y)^{p-1} \right \rbrack.
\\]
By cancellation by outer expectation over $p_{xy}$ similar to that seen above, the above becomes
\\[
    = 
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack f(x) \ W f(y)^{p-1} \right \rbrack. \label{eq:4}\tag{4}
\\]
Defining $g(y) := Wf(y)^{p-1}$, we first derive $g \ \in L_{p'}(y)$.
Since
\\[
    \lVert g(y) \rVert_{p'}^{p'}
    =
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack (W f(y)^{p-1})^{p'}  \right \rbrack
    =
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack W f(y)^{p-1 \frac{p}{p-1}}  \right \rbrack
    =
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack W f(y)^{p} \right \rbrack ,
\\]
applying the contractivity, i.e., Eq.$\,$($\ref{eq:1}$), yields
\\[
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack W f(y)^{p} \right \rbrack
    \leq 
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack f(x)^{p} \right \rbrack
    \lt 
    \infty ,
\\]
where the last inequality comes from the assumption of $f \in L_{p}(x)$,
stating $g \ \in L_{p'}(y)$.

Since $Wf(y)^{p-1} \ \in L_{p'}(y)$, we can apply the assumption of Eq.$\,$($\ref{eq:3}$) to Eq.$\,$($\ref{eq:4}$), yielding
\\[
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack f(x) \ W f(y)^{p-1} \right \rbrack
    \leq 
    \lVert f \rVert_{q} \lVert Wf^{p-1} \rVert_{p'}.
\\]
The LHS is simply
\\[
    \mathop{\mathbb{E}}\_{x,y \sim p_{xy}} \left \lbrack W f(y)^{p} \right \rbrack
    =
    \lVert Wf \rVert_{p}^{p}.
\\]
Regarding the RHS, we know that
\\[
    \lVert Wf^{p-1} \rVert_{p'}
    = 
    \left \( \int |Wf^{p-1}|^{p'} \right \)^{\frac{1}{p'}}
    = 
    \left \( \int |Wf|^{(p-1)p'} \right \)^{\frac{1}{p'}}
    = 
    \left \( \int |Wf|^{p} \right \)^{\frac{1}{p'}}
    = 
    \left \( \int |Wf|^{p} \right \)^{\frac{1}{p} \frac{p}{p'}}
    = 
    \lVert Wf \rVert_{p}^{\frac{p}{p'}}.
\\]
Putting them togather, we obtain
\\[
    \lVert Wf \rVert_{p}^{p}
    \leq
    \lVert f \rVert_{q} \lVert Wf \rVert_{p}^{\frac{p}{p'}}.
\\]
It is equal to
\\[
    \lVert Wf \rVert_{p}^{p - \frac{p}{p'}}
    \leq
    \lVert f \rVert_{q},
\\]
    and since $p - \frac{p}{p'} = p - p \frac{p-1}{p} = 1$, it becomes
\\[
    \lVert Wf \rVert_{p} \leq \lVert f \rVert_{q}.
\\]
The proof of the direction from Eq.$\,$($\ref{eq:2}$) to Eq.$\,$($\ref{eq:3}$) is done.


In this post, we saw the $1$-dimensional case.
We will see the extension to the $n$-dimensional case in the next post. 

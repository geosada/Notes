---
layout: post
title: "Gaussian Equivalence Principle"
---

When we apply an entrywise nonlinear function $\sigma$ to a matrix $X$, the resulting matrix $\sigma(X)$ no longer has independent entries. 
However, if $X$ is a Wigner matrix such as Gaussian Orthogonal Ensemble (GOE), then, in the large-$N$ limit, the spectrum of $\sigma(X)$ is equivalent to that of a simple affine model.
Concretely,
\\[
\mu_{\sigma(X)}\;\Longrightarrow\;\mu_{\alpha X + \beta Z}
\quad(N\to\infty),
\\]
where $\mu_M$ denotes the empirical spectral distribution (ESD) of $M$, $Z$ is an independent GOE, and $\alpha,\beta$ depend only on $\sigma$.  This asymptotic behavior is the Gaussian Equivalence Principle (GEP).


### Empirical Spectral Distribution
The empirical spectral distribution ESD of $N \times N$ matrix $M$ is, loosely speaking, the histogram (or probability measure) that places equal weight on each of its $N$ eigenvalues $\lambda\_{i}$.
Concretely, the ESD is the measure
\\[
	  \mu\_M = \frac1N\sum\_{i=1}^N\delta\_{\lambda\_i}.
\\]
Like
\\[
    \int_{\mathbb{R}} f(x)\,\delta\_{x_0}(\mathrm{d}x) = f(x_0)
\\]
for for a continuous test‐function $f$,
the Dirac delta $\delta\_{\lambda\_i}$ picks out the value at $\lambda\_i$ when we integrate against it.
Dividing by $N$ makes sure that $\mu_{M}$ becomes a probability measure on $\mathbb{R}$,
\\[
    \mu_{M}(\mathbb{R}) = \frac{1}{N}\sum\_{i=1}^{N}1 = 1.
\\]


### Entrywise vs. Spectral Function

There are two ways to apply a scalar function $\sigma$ to a matrix $X\in\mathbb{R}^{N\times N}$:

- Spectral (functional) calculus 

   If $X$ is diagonalizable as $X=Q\Lambda Q^T$, then  
   \\[
     \sigma(X)
     :=Q\,\sigma(\Lambda)\,Q^T,
     \quad
     \sigma(\Lambda)={\rm diag} \bigl(\sigma(\lambda\_i)\bigr).
   \\]
   By construction $\sigma(UXU^T)=U\sigma(X)U^T$ exactly for any orthogonal $U$.  No randomness or invariance is needed.

- Entrywise application 

   If we define  
   \\[
     \bigl[\sigma(X)\bigr]\_{ij}
     :=\sigma\bigl(X\_{ij}\bigr),
   \\]
   then, in general,  
   \\[
     \sigma(UXU^T)\neq U\,\sigma(X)\,U^T.  
   \\]
   But if $X$ is orthogonally invariant ($X\overset{d}{=}U X U^T$), then  
   \\[
     \sigma(X)
     \;\overset{d}{=}\;
     \sigma(U X U^T)
     \;\overset{d}{=}\;
     U\,\sigma(X)\,U^T,
   \\]
   so $\sigma(X)$ is distributionally invariant under conjugation, which suffices to control its ESD.

In the GEP, $\sigma$ is applied entrywise, but the orthogonal invariance of the underlying
$X$ grants that $\sigma(X)$ is distributionally invariant under conjugation, allowing us to reduce spectral questions about the messy entrywise map to questions about diagonal eigenvalue transforms.

# Hermite Expansion

In $\mathbb{R}^n$, two vectors $u,v$ are orthogonal if their dot‐product $u\cdot v=0$.
A real function $f(x)$ can be thought of as an infinite‐dimensional “vector” of values.
we give a notion of “length” and “angle” of functions by defining an inner product
\\[
  \langle f,g\rangle
  =\int_{-\infty}^{\infty} f(x)\,g(x)\,\varphi(x)\,dx,
\\]
where $\varphi(x)$ is a weight function (e.g., the standard Gaussian density).
If $\langle f,g\rangle=0$, we call $f$ and $g$ orthogonal in this infinite‐dimensional function space—it does **not** mean $f(x)\,g(x)=0$ pointwise, but that their weighted integral vanishes.

> **Example (Fourier modes):**  On $[-\pi,\pi]$ with weight $\varphi(x)=1$, $f(x)=\sin(x)$ and $g(x)=\cos(x)$ satisfy
> \\[
> \langle f,g\rangle
> = \int_{-\pi}^\pi \sin(x)\cos(x)\,dx
> = \frac12\int_{-\pi}^\pi \sin(2x)\,dx
> =0.
> \\]

### Hermite Polynomials

We can write a periodic function $f(\theta)$ in a Fourier series
$\displaystyle f(\theta)=\sum\_{m=-\infty}^\infty c_m e^{im\theta}$
because the family $\{e^{im\theta}\}$ is orthogonal under the uniform weight on $[0,2\pi]$.
Likewise, if $\sigma(z)$ is square–integrable under the Gaussian weight, i.e.
\\[
  \int_{-\infty}^\infty \sigma(z)^2\,\varphi(z)\,dz <\infty,
  \quad\varphi(z)=\frac{e^{-z^2/2}}{\sqrt{2\pi}},
\\]
then $\sigma(z)$ admits a **Hermite expansion** in the orthogonal family $\{He\_k(z)\}\_{k=0}^\infty$, where the probabilists’ Hermite polynomials satisfy
\\[
  \langle He\_m,\,He\_n\rangle
  = \int_{-\infty}^{\infty} He\_m(z)\,He\_n(z)\,\varphi(z)\,dz
  = n!\,\delta\_{m n}.
\\]
Hence $\{He\_k(z)/\sqrt{k!}\}\_{k\ge0}$ is an orthonormal basis of $L^2(\mathbb{R},\varphi)$, and by standard Hilbert‐space theory
\\[
  \sigma(z)
  = \sum\_{k=0}^{\infty} c\_k\,\frac{He\_k(z)}{\sqrt{k!}}
  \;=\;
  \sum\_{k=0}^{\infty} a\_k\,He\_k(z),
  \quad
  a\_k = \frac{c\_k}{\sqrt{k!}}.
\\]
Concretely the first few are
\\[
  He\_0(z)=1,\quad
  He\_1(z)=z,\quad
  He\_2(z)=z^2 - 1,\quad
  He\_3(z)=z^3 - 3z,\quad z\sim\mathcal N(0,1).
\\]
Because of orthonormality,
\\[
  c\_k
  = \bigl\langle\sigma,\;\frac{He\_k}{\sqrt{k!}}\bigr\rangle
  = \int \sigma(z)\,\frac{He\_k(z)}{\sqrt{k!}}\,\varphi(z)\,dz
  = \frac{1}{\sqrt{k!}}\,\mathbb{E}\bigl[\sigma(z)\,He\_k(z)\bigr],
\\]
so
\\[
  a\_k
  = \frac{1}{k!}\,\mathbb{E}\bigl[\sigma(z)\,He\_k(z)\bigr].
\\]
In particular
\\[
  a\_1 = \mathbb{E}\bigl[\sigma(z)\,z\bigr],
  \quad
  a\_2 = \tfrac1{2}\,\mathbb{E}\bigl[\sigma(z)\,(z^2-1)\bigr].
\\]

# Gaussian Equivalence Principle

Let $\sigma$ be the elementwise nonlinear function and $X \in \mathbb{R}^{N\times N}$ be a GOE. 
Setting $Y\_N = \sigma(X)$ and thus
\\[
  Y\_N = \frac{1}{\sqrt N}\,\sigma\bigl(\sqrt N\,X\_N\bigr),
\\]
the GEP says that the ESD of $Y\_N$ is equal to $\alpha X + \beta$ as $N \to \infty$.
To show this, we use trace moments matching.

### Trace‑Matching via Moments

A probability measure $\mu$ on a bounded interval is uniquely determined by its moments  
\\[
    m\_\ell=\int x^\ell\,d\mu(x).  
\\]
For an $N\times N$ matrix $M$ with ESD $\mu\_M$,  
\\[
  m\_\ell \;=\;\int x^\ell\,d\mu\_M(x)
  = \frac1N\sum\_{i=1}^N\lambda\_i^\ell
  = \frac{1}{N}\mathrm{tr}\bigl(M^\ell\bigr).
\\]
Hence to show $\mu_{Y\_N}$ and $\mu_{\alpha X\_N+\beta Z\_N}$ coincide, it suffices to prove for each $\ell$
\\[
  \frac{1}{N}\mathbb{E}\bigl[\mathrm{tr}(Y\_N^\ell)\bigr]
  \;-\;
  \frac{1}{N}\mathbb{E}\bigl[\mathrm{tr}\bigl((\alpha X\_N+\beta Z\_N)^\ell\bigr)\bigr]
  \;\xrightarrow[N\to\infty]{}\;0.
\\]
The derivation is as follows.

#### 1. Entrywise expansion
The entry of $Y\_N$ is
\\[
  Y\_N(i,j)
  = \frac{1}{\sqrt N}\sum\_{k=0}^\infty a\_k\,He\_k\bigl(\sqrt N\,X\_{ij}\bigr).
\\]
Grouping terms gives
\\[
  Y_{N}(i,j)
  = \underbrace{\frac{a\_0}{\sqrt N}}\_{k=0}
  \+ \underbrace{a\_1\,X\_{ij}}\_{k=1}
  \+ \underbrace{a\_2\Bigl(\sqrt N\,X\_{ij}^2-\frac1{\sqrt N}\Bigr)}\_{k=2}
  \;+\;\sum\_{k\ge3}\frac{a\_k}{\sqrt N}\,He\_k\bigl(\sqrt N\,X\_{ij}\bigr).
\\]

The $k=0$ term vanishes. 
The $k=0$ contribution is $\tfrac{a\_0}{\sqrt N}\,11^T$, where $1\,1^T$ is the all‑ones matrix.
This rank-1 matrix has one eigenvalue
$a\_0\sqrt N$ since $1\,1^T$ has a single nonzero eigenvalue $N$ and $N-1$ zero eigenvalues.
Thus it  contributes a spike at $\lambda\_i = a\_0\sqrt N$ in the ESD 
  $\displaystyle \mu_{Y\_N}=\tfrac1N\sum\_i\delta\_{\lambda\_i(Y\_N)}$,
but the single spike carries mass $1/N$, so it vanishes as $N\to\infty$.
The ESD ignores a single spike.

The $k=1$ term contributes
   \\[
     a\_1\,X\_{ij}
     = \alpha\,X\_{ij},
     \quad
     \alpha = \mathbb{E}\bigl[\sigma(Z)\,Z\bigr],
     \quad Z\sim\mathcal N(0,1).
   \\]

Defining 
\\[
     Q\_{ij}
     := \frac{1}{\sqrt N}\,He\_2\bigl(\sqrt N\,X\_{ij}\bigr)
     = \sqrt N\,X\_{ij}^2 - \frac{1}{\sqrt N},
\\]
the $k=2$ term can be written
   \\[
     a\_2\,Q_{ij}.
   \\]
Hence  
\\[
    Y\_N = \alpha\,X + a\_2\,Q + R,
\\]
where $R$ collects the Hermite–$k$ pieces for $k\ge3$.


#### 2. Matrix‐power formula

For any matrix $M$,
\\[
(M^\ell)\_{i j}
=\sum\_{i_{2},\dots,i_{\ell}}
M_{i\,i_{2}}\;M_{i_{2}\,i_{3}}\;\cdots\;M_{i_{\ell}\,j},
\\]
so
\\[
\mathrm{tr}(M^\ell)
=\sum\_{i_{1},i_{2},\dots,i_{\ell}}
M_{i_{1}i_{2}}\;M_{i_{2}i_{3}}\;\cdots\;M_{i_{\ell}i_{1}}.
\\]


#### 3. Expansion of $\mathrm{tr}(Y\_N^\ell)$

\\[
\mathrm{tr}\bigl(Y\_N^\ell\bigr)
=\sum\_{i_{1},\dots,i_{\ell}}
\bigl(\alpha X+a\_2 Q+R\bigr)\_{i_1 i_2}\,
\cdots\,
\bigl(\alpha X+a\_2Q+R\bigr)\_{i_\ell i_1}.
\\]

Taking expectations and dividing by $N$ kills almost every term.  Only two types survive:

1. Pure linear $(\alpha X)^\ell$.
2. Pairings of the $Q$–factors in even numbers (Wick contractions).


#### 4. Wigner‐pairing and combinatorics

By the definition of Wigner matrix, $\mathbb{E}[X\_{ij}]=0,\;\text{Var}[X\_{ij}]=1/N$,
so even if the true law of $X\_{ij}$ is not Gaussian, we treat it “as if” it were Gaussian and consider its fourth moment is
  $\mathbb{E}[X\_{ij}^4]\approx3/N^2$.
Using them, we know
\\[
 \mathbb{E}[Q_{ij}] = \sqrt N \mathbb{E}[X\_{ij}^2] - \frac{1}{\sqrt N} = \sqrt N \bigl( \text{Var}[X\_{ij}] - \mathbb{E}[X\_{ij}]^2 \bigr) - \frac1N = 0.
\\]
Also,
  \\[
    \mathbb{E}[Q_{ij}]=0,
    \quad
    \mathbb{E}[Q_{ij}^2]
    =N\,\mathbb{E}[X\_{ij}^4]
     \;-\;2\,\mathbb{E}[X\_{ij}^2]
     \;+\;\frac1N
    =\frac{2}{N},
  \\]
  and if $\{i,j\}\neq\{k,\ell\}$, then $Q_{ij}$ and $Q_{k\ell}$ are independent so
  $\mathbb{E}[Q_{ij}Q_{k\ell}]= 0・0 = 0$.

- **Pairing rule:**  Only terms where each factor appears exactly twice can survive expectation.  Each such pair forces one index‐identification, so $\ell$ factors → $\ell/2$ pairings → $\ell/2+1$ free sums → factor $N^{\ell/2+1}$.

  > **Example ($\ell=4$)**:
  > $\mathrm{tr}(X^4)=\sum\_{i_1,i_2,i_3,i_4}X\_{i_1i_2}X\_{i_2i_3}X\_{i_3i_4}X\_{i_4i_1}.$
  > A nonzero pairing $\{(1,2),(3,4)\}$ sets $i_1=i_2$ and $i_3=i_4$, leaving 2 free indices plus the trace root → $N^3$.

- **Higher Hermite ($k\ge3$)** require at least $k$ copies to form a nonzero cumulant because of the orthogonality of Hermite expansion, which costs an *extra* $(N^{-1/2})^{\,k-2}$ and yields at best
  \\[
    N^{\ell/2+1}\times(N^{-1/2})^{\ell}\times(N^{-1/2})^{\,k-2}
    =N^{1-\tfrac{k-2}{2}}.
  \\]
  Thus any term with an $R$–factor is $o(N)$ and vanishes after dividing by $N$.

- **Scaling**:
  The size of all the three terms, $X\_{ij}$, $Q_{ij}$, and $R_{ij}$ is $O(N^{-1/2})$ in probability. 
  The size of Wigner matrix entries is $X\_{ij}$ = $O(N^{-1/2})$, 
  since $\mathbb{E}[X\_{ij}^2]  = \text{Var}[X\_{ij}] + \mathbb{E}[X\_{ij}]^2 = 1/N + 0 = 1/N$.
  Similarly, $Q\_{ij} = O(N^{-1/2})$, and from the definition $R\_k = O(N^{-1/2})$.


#### 5. Leading contribution

Only $\alpha X$ and $a\_2 Q$ survive at order $O(N)$, giving
\\[
\frac{1}{N}\mathbb{E}[\mathrm{tr}(Y\_N^\ell)]
=
\frac{1}{N}\mathbb{E}\bigl[\mathrm{tr}((\alpha X+a\_2Q)^\ell)\bigr]
+o(1).
\\]


#### 6. Matching $Q$ to GOE noise

Define $\beta=a\_2\sqrt2$ and let $Z\_N\sim\mathrm{GOE}$.  One checks
$\text{Var}((\beta Z)\_{ij})=a\_2^2\text{Var}(Q_{ij})$ and zero off-diagonal covariances, so **term–by–term** the surviving pairings in
$\mathrm{tr}((\alpha X+a\_2Q)^\ell)$
match those in
$\mathrm{tr}((\alpha X+\beta Z)^\ell)$.


#### 7. Conclusion by method of moments

For each fixed $\ell$,
\\[
\frac{1}{N}\mathbb{E}\bigl[\mathrm{tr}(Y\_N^\ell)\bigr]
-\frac{1}{N}\mathbb{E}\bigl[\mathrm{tr}((\alpha X+\beta Z)^\ell)\bigr]
\;\to\;0.
\\]
Matching **all** moments implies
$\mu_{Y\_N}\xrightarrow{w}\mu_{\alpha X+\beta Z}$ as $N\to\infty$,
which is the **Gaussian Equivalence Principle**.


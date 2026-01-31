---
layout: post
title: "Gaussian Orthogonal Ensemble"
---

As a preparation for the Gaussian Equivalence Principle (GEP), we see the Gaussian Orthogonal Ensemble (GOE) and its key property, orthogonal invariance, in this post.

# Orthogonal Invariance

Letting $X$ be an $N \times N$ random matrix and $U$ be orthogonal matrix,
conjugating 
\\\[
    X \mapsto UXU^{T}
\\\]
is the action of changing to a new orthonormal basis.
We say a random‐matrix law is orthogonally invariant if for every fixed $U$ in the orthogonal group $O(N)$, the random matrices $X$ and $UXU^{T}$ have the same distribution.
The invariance of eigenvalue distribution under conjugation with any $U$ implies the matrix law depends only on its eigenvalues (up to distribution).
This distribution of the eigenvalues of a random matrix ensemble is called the empirical spectral distribution (ESD).
Beginning with the definition of conjugation, we introduce GOE (Gaussian Orthogonal Ensemble) as the representative instance of orthogonal invariance.


### Rotation and conjugation

Let $U \in O(N)$ be an orthogonal matrix and $x$ be a vector in $\mathbb{R}^{N}$.
If we rotate the basis by $U$, a vector that was $x$ in the old basis has coordinates $x' = Ux$ in the new basis.
In this rotation operation, vectors lengths and angles, i.e., dot-products, are preserved, $\|x'\|=\|x\|$ and $x'\cdot y' = x\cdot y$.
A matrix $X$ represents a linear map $x \mapsto z = Xx$.
To see how $X$ acts in the rotated basis, we express the map in terms of the new coordinates $x'$ and $z'$:
\\\[
    z' = Uz = U(Xx).
\\\]
Since $x' = Ux$, we have $x = U^{-1}x' = U^{T}x'$. Substituting this into the equation:
\\\[
    z' = U X (U^{T}x') = (UXU^{T})x'.
\\\]
Hence, in the rotated basis, the matrix representing the same linear map is $X' = UXU^{T}$.
Conjugation by $U$ doesn’t change what the map does, only how you describe it in coordinates that have been rotated by $U$.
Conjugation changes each eigenvector $v$ of $X$ by $Uv$, but preserves its spectrum, i.e., leave each eigenvalue $\lambda$ unchanged.
For $X = Q\Lambda Q^{T}$, 
\\\[
    UXU^{T} = (UQ)\Lambda(UQ)^{T},
\\\]
the eigenvalues $\Lambda$ stay the same; only the basis $Q$ changes to $UQ$.


### No preferred direction

We say a matrix‐ensemble is conjugation‐invariant (or orthogonally‐invariant) if, for every fixed $U\in O(N)$, the random matrices $X$ and $UXU^{T}$ have the same law:
\\\[
  \mathcal L(X) \;=\;\mathcal L\bigl(U\,X\,U^T\bigr).
\\\]

By analogy with vector rotation‐invariance $x \mapsto Ux$ (which means $x$ and $Ux$ have the same distribution, so the law depends only on $|x|$), we reserve “rotation‐invariance” for distributions on vectors and use conjugation‐invariance (or orthogonal‐invariance) for the matrix‐version under $X \mapsto U X U^{T}$.
Equivalently, conjugation‐invariance says the law of $X$ depends only on its eigenvalues (the spectrum) and not on its eigenvectors.
Note that $\mathcal L(X)$ is a measure on all of $\mathbb{R}^{N\times N}$, not just on the eigenvalues, but because conjugation leaves eigenvalues unchanged—only the eigenvectors are rotated—orthogonal‐invariance implies the ensemble’s spectral measure (the ESD) is unchanged by any basis rotation.
Also note that orthogonal-invariance says nothing about the dispersion of the eigenvalues themselves; it never means that all $\lambda_{i}$ are equal.

In practice (e.g.\ GOE or any Wigner ensemble) the eigenvalues spread out according to a nontrivial law such as the semicircle distribution.
Because of this full invariance, virtually every global spectral statistic (moments, Stieltjes transform, free convolution, etc.) depends only on the ESD of $X$ and not on any subtle eigenvector correlations.


# Matrix isotropy

The GOE is a random‐matrix ensembles.
While a random matrix is a single matrix‐valued random variable,
a random‐matrix ensemble is the entire family (or “ensemble”) of random matrices together with the rule for sampling them.
Its defining law can be compactly expressed via the Frobenius inner product.

### Frobenius inner product

To talk about “lengths” and “angles” (and hence isotropy), we need an inner product in general.
The Frobenius inner product is defined as
\\\[
  \langle A,B\rangle_{F}
  = \mathrm{tr}(A\,B)
  = \sum_{i,j=1}^{N} A_{ij}\,B_{ij}
  = \mathrm{vec}(A)\cdot\mathrm{vec}(B),
\\\]
which induces the norm $\|A\|\_{F} = \sqrt{\mathrm{tr}(A^{2})}$, the natural “length” of a matrix.
Using this metrics we identify $x \in \mathbb{R}^{N\times N}$ being spherical or isotropic, just like a standard multivariate normal in $\mathbb{R}^{N}$.
The subspace of real symmetric $N\times N$ matrices has dimension
$\frac{N(N+1)}{2}$.
A convenient orthonormal basis (w.r.t. $\langle\cdot,\cdot\rangle_{F}$) is
\\\[
  E^{(i,i)} = E_{ii},
  \quad
  E^{(i,j)} = \frac{E_{ij}+E_{ji}}{\sqrt2}\quad(i\<j),
\\\]
where $E_{ij}$ has a 1 in the $(i,j)$ slot and 0 elsewhere.
In particular, isotropy requires
\\\[
  \mathbb{E}\bigl[\langle X,E^{(a)}\rangle_{F}\,
         \langle X,E^{(b)}\rangle_{F}\bigr]
  =\sigma^{2}\,\delta_{ab}
  \quad\forall\,a,b.
\\\]

### Requirements of isotropy
A multivariate Gaussian in $\mathbb{R}^{N}$ is called spherical (or isotropic) if its covariance matrix is a scalar multiple of the identity.
Equivalently, via the $\mathrm{vec}$ map $X \mapsto \mathrm{vec}(X) \in \mathbb{R}^{N^{2}}$ (or $\mathbb{R}^{N(N+1)/2}$ for symmetric $X$), we ask:
Is $\mathrm{vec}(X) \sim \mathcal{N}(0,\sigma^{2} \mathbb{I})$?
If so, $X$ is an isotropic (spherical) Gaussian in matrix‐space, i.e., no preferred direction.
The coordinate of $X$ in direction $E^{(a)}$ is expressed by the Frobenius inner product as
\\[
\langle X,\,E^{(a)}\rangle_{F}
= \mathrm{tr}\bigl(X\,E^{(a)}\bigr).
\\]
Isotropy (a truly spherical Gaussian) demands that the covariance between two directions $a,b$ is
\\[
\mathbb{E}\bigl[\langle X,E^{(a)}\rangle_{F}\,\langle X,E^{(b)}\rangle_{F}\bigr]
= \sigma^2\,\delta_{ab},
\\]
so that every orthonormal direction has the same variance $\sigma^2$.
A convenient orthonormal basis of the symmetric‑matrix space is
\\[
E^{(i,i)} = E_{ii},
\quad
E^{(i,j)} = \frac{E_{ij}+E_{ji}}{\sqrt2}
\quad(i\<j),
\\]
where $E_{ij}$ has a 1 in the $(i,j)$ slot and 0 elsewhere.

- **Off‑diagonal direction:**
  \\[
    \langle X,\,E^{(i,j)}\rangle_{F}
    = \frac{1}{\sqrt2}\,(X_{ij}+X_{ji})
    = \sqrt2\,X_{ij},
  \\]
  hence
  \\[
    \mathrm{Var}\bigl(\langle X,E^{(i,j)}\rangle_{F}\bigr)
    = 2\,\mathrm{Var}(X_{ij})
    = \frac{2}{N}.
  \\]

- **Diagonal direction:**
  \\[
    \langle X,\,E^{(i,i)}\rangle_{F}
    = X_{ii}.
  \\]
  To make all directions isotropic, set
  $\displaystyle \mathrm{Var}(X_{ii})=\frac{2}{N}.$

Consequently, the Frobenius norm of a symmetric $X$ is
\\[
\|X\|\_{F}^2
= \sum_{i,j}X_{ij}^2
= \mathrm{tr}(X^2)
= \sum_i X_{ii}^2 + 2\sum_{i\<j}X_{ij}^2,
\\]
and since $\mathbb{E}[X\_{ij}]=0$,
\\[
\mathbb{E}\bigl[\|X\|\_{F}^2\bigr]
= \sum\_{i} \mathrm{Var}(X\_{ii})
  \+ 2\sum\_{i\<j} \mathrm{Var} (X\_{ij}),
\\]
so each independent coordinate contributes its variance to the total.

# GOE is Orthogonally Invariant

We derive the GOE density $p(X)$.
The entries of $X$ are chosen to satisfy isotropy:
\\[
X_{ij}\sim\mathcal{N} \bigl(0,\frac1N\bigr)\ (i\<j),
\quad
X_{ii}\sim\mathcal{N} \bigl(0,\frac2N\bigr).
\\]
Hence the joint density factorizes as
\\[
p(X) \;\propto\;
\prod_{i\<j} \exp\ \Bigl(-\frac{N}{2}\,X_{ij}^2\Bigr)
\;\times\;
\prod_{i=1}^{N} \exp\ \Bigl(-\frac{N}{4}\,X_{ii}^2\Bigr)
=
\exp \Bigl(-\frac{N}{2}\sum_{i\<j}X_{ij}^2 \;-\;\frac{N}{4}\sum_{i}X_{ii}^2\Bigr).
\\]

Notice

\\[
\sum_{i\<j}X_{ij}^2 \;+\;\sum_{i\<j}X_{ji}^2\;+\;\sum_{i}X_{ii}^2
\;=\;
\sum_{i,j}X_{ij}^2
\;=\;
\mathrm{tr}\bigl(X^2\bigr).
\\]

Since $X_{ij}=X_{ji}$, one has
$\displaystyle\sum_{i\<j}X_{ij}^2 = \frac12\sum_{i\neq j}X_{ij}^2$.  Therefore

\\[
-\frac{N}{2}\sum_{i\<j}X_{ij}^2
\;-\;
\frac{N}{4}\sum_{i}X_{ii}^2
\;=\;
-\frac{N}{4}\sum_{i,j}X_{ij}^2
\;=\;
-\frac{N}{4}\mathrm{tr}\bigl(X^2\bigr).
\\]

Thus, the desity is 

\\[
p(X)
\;\propto\;
e^{-\frac{N}{4}\mathrm{tr}(X^2)}.
\\]


Since $\mathrm{tr}\bigl((U X U^T)^2\bigr)=\mathrm{tr}(X^2)$ for any $U\in O(N)$, it follows that
\\[
p(U X U^T) = p(X),
\\]
i.e., the GOE law is orthogonally invariant—it depends only on the eigenvalues of $X$.

### GOE $\subset$ Wigner
GOE is the member of the broader Wigner class.
A Wigner matrix is any $N\times N$ real symmetric ensemble with
\\[ 
    \mathbb{E}[X_{ij}]=0, 
    \quad 
    \text{Var}(X_{ij})=1/N, 
    \quad 
    \text{Var}(X_{ii})=\tau^2/N,
\\]
plus mild higher‐moment bounds. The GOE corresponds to the Gaussian case $\tau^2=2$.
Wigner’s Semicircle Law then tells you that, as $N \rightarrow \infty$, the empirical spectral distribution of any such Wigner matrix converges to the semicircle density

\\[
  \rho_{\mathrm{sc}}(x)
  = \frac{1}{2\pi}\sqrt{4 - x^2}\,\mathbf{1}\_{|x|\le2}.
\\]

Because of the Gaussian law on each entry, the GOE has the extra property of exact invariance under $X \mapsto UXU^{T}$.
No other Wigner ensemble (with non-Gaussian entries) enjoys that full rotational invariance.

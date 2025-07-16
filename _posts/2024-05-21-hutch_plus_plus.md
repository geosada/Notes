---
layout: post
title: "Hutch++: Efficient Stochastic Trace Estimation"
---
Estimating the trace of large matrices efficiently is a key primitive in many machine learning tasks—e.g., computing divergence in continuous normalizing flows or approximating log-determinants in Gaussian processes. 
Hutchinson’s estimator uses random probes to give unbiased trace estimates but requires $O\bigl(1/\varepsilon^2 \bigr)$ matrix–vector multiplications to achieve relative error $\varepsilon$.
Hutch++ enhances this to $O\bigl(1/\varepsilon \bigr)$ by combining a one-time low-rank deflation with a few classical Hutchinson probes.
## 1. Scoop-and-Weigh 

Imagine a jar of mixed coins—pennies, nickels, dimes, and quarters—and you want its total value without counting each coin:

1. **Vanilla Hutchinson**: scoop random handfuls and weigh them; each scoop is noisy, so many scoops are needed.
2. **Hutch++**: first pull out and count all quarters (the “heavy hitters”) exactly, then take a few handfuls of the remaining small-change. Fewer handfuls suffice because the leftovers are more uniform.

By removing the largest contributors first, Hutch++ drastically reduces the number of random probes.

---

## 2. Algorithm

Let $A \in \mathbb{R}^{n\times n}$ be symmetric, with trace $\tau = \mathrm{tr}(A)$. We target relative error $\epsilon$. Hutch++ runs in two stages:

### 2.1 Low-Rank Deflation ("Pulling Out the Quarters")

1. **Sketch matrix**: draw $S \in \mathbb{R}^{n\times m_{1}}$ with $m_{1} = O(1/\epsilon)$ random Gaussian columns.
2. **Amplify**: compute
   \\[
   Y = A S,
   \\]
   using $m_{1}$ mat–vecs. Each column $y_{i}=A s_{i}$ is biased toward top eigen-directions.

   - **Why?**  Write each random $s_{i}$ in the eigenbasis $\{v_{j}\}$:
     \\[
       s_{i} = \sum_{j=1}^{n} (v_{j}^\top s_{i})\,v_{j} = \sum_{j=1}^{n} c_{j}v_{j}, \quad c_{j} = v_{j}^\top s_{i},
     \\]
     and then
     \\[
       y_{i} = A s_{i} = V\Lambda V^\top s_{i} = V\Lambda V^\top (\sum_{j=1}^{n} c_{j}v_{j}) = V\Lambda (c_{1}, c_{2}, ...)^\top = \sum_{j=1}^{n} 
       \lambda_{j}c_{j} v_{j} = \sum_{j=1}^{n} \lambda_{j}(v_{j}^\top s_{i})\,v_{j}.
     \\]
     So the coefficient along eigenvector $v_{j}$ in $y_{j}$ is $\lambda_{j} c_{j}$, that is, 
     each direction $v_{j}$ in the output $y_{j}$ is amplified by the factor $\lambda_{j}$.
     The top-eigenvector directions get “stretched” more, making them stand out in the collection of all $y_{j}$.
     So each $y_{j}$ tends to “point toward” the subspace spanned by the top eigenvectors.

3. **Orthonormalize**: perform QR on $Y$ to get $Q \in \mathbb{R}^{n\times m_{1}}$ with orthonormal columns.  Theory shows
   \\[
     \mathrm{span}(Q) \approx \text{span of top-}m_{1}\text{ eigenvectors of }A.
   \\]

4. **Exact trace of low-rank part**: form the small matrix
   \\[
     B = Q^\top (A Q),
   \\]
   then compute
   \\[
     \tau_{\mathrm{low}} = \mathrm{tr}(B).
   \\]

   This captures the “quarters.”  The **residual** matrix is
   \\[
     R = P A P,
     \quad
     P = I - QQ^\top,
   \\]
   where $P$ is an **orthogonal projector** onto the subspace **left over** after removing span($Q$).  A projector satisfies
   \\[
     P^{2} = P,
     \quad
     P^{\top} = P.
   \\]
   Intuitively, for any vector $x$, $P x = x - QQ^\top x$ **subtracts out** its projection onto span($Q$), leaving the component orthogonal to those top directions.

### 2.2 Residual Estimation ("Few Handfuls of Small Coins")

Perform $m_{2}=O(1)$ Hutchinson probes on $R=PAP$:

1. **Project** a random $v_{j}$:
   \\[
     z_{j} = P v_{j}.
   \\]
2. **Probe**:
   \\[
     X_{j} = z_{j}^\top (A z_{j}) = z_{j}^\top R\,z_{j}.
   \\]
3. **Average** for an unbiased estimate:
   \\[
     \tau_{\mathrm{res}} = \frac{1}{m_{2}}\sum_{j=1}^{m_{2}} X_{j} \approx \mathrm{tr}(R).
   \\]

### 2.3 Combine

The final estimate is
\\[
  \widehat{\tau} = \tau_{\mathrm{low}} + \tau_{\mathrm{res}},
\\]
using $O(1/\epsilon)$ mat–vecs total instead of $O(1/\epsilon^{2})$.

---

## 3. Why It Works: Capturing the Top Eigenspace

1. **Eigen-decomposition**:
   \\[
     A = V\Lambda V^\top,
     \quad
     \Lambda = \mathrm{diag}(\lambda_{1} \ge \lambda_{2} \ge \dots \ge \lambda_{n}).
   \\]
2. **Random sketch** columns $s_{i}$ expand as above; $y_{i}=As_{i}$ **amplifies** large $\lambda_{j}$.
3. **QR → $Q$**: orthonormalizing $Y$ extracts the dominant subspace.

With $m_{1}=O(1/\epsilon)$,
\\[
  \mathrm{tr}(P A P) = \sum_{j>m_{1}} \lambda_{j} \le \epsilon \sum_{j=1}^{n} \lambda_{j} = \epsilon\tau.
\\]

---

## 4. Detailed Error Analysis

- **Low-rank error** $\Delta_{\mathrm{low}}$:
  Ideal deflation misses
  \\[
    \Delta_{\mathrm{low}} = \tau - \tau_{\mathrm{low}} = \mathrm{tr}(P A P) = \sum_{j>m_{1}} \lambda_{j}.
  \\]
  By choosing $m_{1}=O(1/\epsilon)$ via randomized sketches, one ensures
  \\[
    \Delta_{\mathrm{low}} \le \epsilon\tau
  \\]
  with high probability.

- **Residual error**:
  The Frobenius norm of $R$ satisfies
  \\[
    \lVert R\rVert_F \;=\;\lVert P\,A\,P\rVert_F
    \;\le\;\sqrt{\mathrm{rank}(R)}\,\lVert A\rVert_F
    \;=\;O(\sqrt{\epsilon})\,\lVert A\rVert_F.
  \\]
  Thus a constant number $m_{2}$ of probes yields
  \\[
    |\tau_{\mathrm{res}} - \mathrm{tr}(R)| = O(\epsilon\tau).
  \\]

- **Overall**: combining both,
  \\[
    |\widehat{\tau} - \tau| = O(\epsilon\tau),
  \\]
  i.e.\ $\widehat{\tau}=(1\pm O(\epsilon))\,\tau$.

---

## 5. Practical Considerations

- **Spectral decay**: fast decay maximizes gains; flat spectra reduce benefits.
- **Preprocessing cost**: QR on $Y$ and storing $Q$ add overhead—best when $n$ large and $\epsilon$ small.
- **Tuning**: select $m_{1},m_{2}$ to balance deflation vs. sampling.
- **Projector recap**: $P=I-QQ^\top$ satisfies $P^{2}=P$, projecting out span($Q$).

---

## 6. Conclusion

**Hutch++** combines low-rank deflation (“lift out the quarters”) and Hutchinson probes (“handfuls of coins”) to reduce trace-estimation from $O(1/\epsilon^{2})$ to $O(1/\epsilon)$, offering an efficient, unbiased, scalable method for high-accuracy trace estimation.

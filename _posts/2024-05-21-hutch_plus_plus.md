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

## 2. Step-by-Step Algorithm

Let \(A \in \mathbb{R}^{n\times n}\) be symmetric, with trace \(\tau = \mathrm{tr}(A)\). We target relative error \(\epsilon\). Hutch++ runs in two stages:

### 2.1 Low-Rank Deflation ("Pulling Out the Quarters")

1. **Sketch matrix**: draw \(S \in \mathbb{R}^{n\times m_{1}}\) with \(m_{1} = O(1/\epsilon)\) random Gaussian columns.
2. **Amplify**: compute the matrix–product
   \[
   Y = A S,
   \]
   using \(m_{1}\) mat-vec products. Each column \(y_{i} = A s_{i}\) emphasizes directions with larger eigenvalues.
3. **Orthonormalize**: perform a thin QR on \(Y\) to obtain \(Q \in \mathbb{R}^{n\times m_{1}}\) with orthonormal columns.  Theory shows \(\mathrm{span}(Q)\) approximates the top-\(m_{1}\) eigenspace of \(A\).
4. **Exact trace of low-rank part**: form the small \(m_{1}\times m_{1}\) matrix
   \[
   B = Q^\top (A Q),
   \]
   then compute its trace
   \[
   \tau_{\mathrm{low}} = \mathrm{tr}(B),
   \]
   capturing the dominant contribution to \(\tau\).

After this, the **residual** trace under the projector

\[
P = I - Q Q^\top
\]

satisfies with high probability:

\[
\mathrm{tr}(P A P) \le \epsilon \, \tau.
\]

### 2.2 Residual Estimation ("Few Handfuls of Small Coins")

Perform \(m_{2} = O(1)\) Hutchinson probes on the residual matrix \(P A P\):

1. **Project** a random vector \(v_{j}\):
   \[
   z_{j} = P v_{j}.
   \]
2. **Probe**:
   \[
   X_{j} = z_{j}^\top (A z_{j}) = z_{j}^\top (P A P) z_{j},
   \]
3. **Average**:
   \[
   \tau_{\mathrm{res}} = \frac{1}{m_{2}} \sum_{j=1}^{m_{2}} X_{j},
   \]
   an unbiased estimate of \(\mathrm{tr}(P A P)\).

### 2.3 Combine

The final trace estimate is

\[
\widehat{\tau} = \tau_{\mathrm{low}} + \tau_{\mathrm{res}}.
\]

The total cost is \(O(1/\epsilon)\) mat-vecs, improving over \(O(1/\epsilon^{2})\) for vanilla Hutchinson.

---

## 3. Why It Works: Capturing the Top Eigenspace

- **Eigen-decomposition**:
  \[
  A = V \Lambda V^\top,
  \quad
  \Lambda = \mathrm{diag}(\lambda_{1} \ge \lambda_{2} \ge \dots \ge \lambda_{n}).
  \]
- **Random sketch**: expand each column \(s_{i}\) as
  \[
  s_{i} = \sum_{j=1}^{n} c_{j} \, v_{j},
  \quad
  c_{j} = v_{j}^\top s_{i}.
  \]
- **Amplification**: applying \(A\) gives
  \[
  y_{i} = A s_{i} = V \Lambda V^\top s_{i} = \sum_{j=1}^{n} \lambda_{j} \, c_{j} \, v_{j},
  \]
  so large eigenvalues yield larger components.
- **QR →** \(Q\)**:** orthonormalizing the columns of \(Y\) extracts a basis approximating the top eigenspace.

With \(m_{1} = O(1/\epsilon)\), one can show:

\[
\mathrm{tr}(P A P) = \sum_{j>m_{1}} \lambda_{j} \le \epsilon \, \sum_{j=1}^{n} \lambda_{j} = \epsilon \, \tau.
\]

---

## 4. Error Analysis

1. **Low-rank error**: if \(Q\) captured the true top \(m_{1}\) eigenvectors,
   \[
   \Delta_{\mathrm{low}} = \sum_{j=m_{1}+1}^{n} \lambda_{j} \le \epsilon \, \tau.
   \]
   Randomized sketching achieves the same bound up to lower-order terms.
2. **Residual variance**:
   \[
   \|P A P\|_{F} \le \sqrt{\epsilon} \, \|A\|_{F},
   \]
   so a constant number of probes gives relative error \(O(\epsilon)\).

Combined:

\[
\widehat{\tau} = (1 \pm O(\epsilon)) \, \tau
\]

with high probability.

---

## 5. Practical Considerations

- **Spectral decay**: rapid eigenvalue decay yields maximum gains; flat spectra diminish benefits.
- **Preprocessing cost**: QR on \(Y\) and storing \(Q\) add overhead—best when \(n\) is large and \(\epsilon\) small.
- **Tuning**: choose sketch size \(m_{1}\) and probe count \(m_{2}\) to balance deflation versus sampling cost.
- **High-confidence**: if pre-committing probes, require \(O(\sqrt{\log(1/\delta)})\) extra factor for failure probability \(\delta\).

---

## 6. Conclusion

**Hutch++** combines a one-time low-rank deflation with a few standard Hutchinson probes to reduce trace-estimation complexity from \(O(1/\epsilon^{2})\) to \(O(1/\epsilon)\). By “lifting out the quarters” first and then taking “handfuls of small coins,” it offers an efficient, unbiased, and scalable solution for high-accuracy trace estimation in machine-learning pipelines.

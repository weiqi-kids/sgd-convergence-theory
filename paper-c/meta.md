# paper-c — Metadata

## Title
A Closed-Loop Theorem for SGD's Noise-Driven Selection in the Non-Interpolation Regime

## Author
Lightman Chang (Independent Researcher) — `lightman.chang@gmail.com`

## Type
Type A — new theorem with rigorous proof.

## Summary
Companion paper-C to the SGD convergence-theory series. While paper-A
treats the interpolation regime via discrete Lyapunov stability and paper-B
catalogues the four failure modes (F1–F4), paper-C is the positive
non-interpolation result: a single closed-loop theorem linking
(I) the Fokker–Planck stationary measure of SGD to a closed-form
Gibbs-type formula in $\Sigma/H$, (II) the leave-one-out generalization
gap to $\Sigma/(H(n-1))$, and (III) the implication that
$\Sigma_A/H_A < \Sigma_B/H_B$ forces both selection of $A$ by SGD and
strictly smaller test-loss bound at $A$.  The paper extends the dynamic
part to multivariate parameter spaces using the Freidlin–Wentzell
quasipotential, with a worked 2D example $V = \theta_1^2 + \theta_2^2 -
\tfrac{1}{2}\theta_1^2\theta_2^2$ in which detailed balance fails but the
Hamilton–Jacobi expansion is computable to order 4.  Reparameterization
invariance of $\Sigma/H$ (and the multivariate $\tr(H^{-1}\Sigma)$) is
proved with the full Jacobian-of-diffeomorphism calculation.

## Key result (one-line)
For SGD with two locally quadratic minima in the non-interpolation regime,
the stationary measure ratio, the LOO gap, and the test-loss bound at each
minimum are all controlled by the single intrinsic quantity $\Sigma/H$;
$\Sigma_A/H_A < \Sigma_B/H_B$ implies SGD selects $A$ AND $A$ generalizes
strictly better, with explicit margin $(\Sigma_B/H_B - \Sigma_A/H_A)/(n-1)
+ O(n^{-2})$.

## Page count / line count
- LaTeX source: 1436 lines
- Compiled PDF: 16 pages

## Target journals (ranked)

| Rank | Journal | Acceptance estimate | Rationale |
|------|---------|---------------------|-----------|
| 1 | JMLR (Journal of Machine Learning Research) | 25–35% | Primary fit: long-form rigorous theory, includes proofs in main text, embraces companion-paper series; matches paper-A and paper-B target. |
| 2 | Information and Inference (Oxford) | 20–30% | Strong fit for SDE / Fokker–Planck-style theory of optimization; values reparameterization-invariance and quasipotential analysis. |
| 3 | SIAM Journal on Mathematics of Data Science (SIMODS) | 20–30% | Welcomes rigorous probabilistic-analysis papers on SGD; would emphasize the Freidlin–Wentzell extension. |
| 4 | Annals of Applied Probability | 10–20% | Reach: would accept if Freidlin–Wentzell content is expanded to a global statement (currently only local 4-th order). |
| 5 | NeurIPS / ICML (full paper track, theory area) | 15–25% | Conference fallback; would require shortening to 9 pages and emphasizing testable predictions. |

## MSC 2020 codes

- **Primary**: 60H10 — Stochastic ordinary differential equations
- Secondary: 62L20 — Stochastic approximation (incl.\ SGD)
- Secondary: 60F10 — Large deviations
- Secondary: 68T07 — Artificial neural networks and deep learning
- Secondary: 49L25 — Viscosity solutions to Hamilton–Jacobi equations
- Secondary: 60J60 — Diffusion processes
- Secondary: 65C05 — Monte Carlo methods (numerical SDE simulation,
  testable predictions in §10.4)

## Submission strategy

1. **Coordinate with paper-A and paper-B.**  Submit all three to JMLR
   together as a "companion series" (JMLR explicitly supports this). If
   JMLR declines paper-C alone, target Information and Inference next.
2. **arXiv preprint first.**  Post all three papers to arXiv `cs.LG` /
   `math.PR` simultaneously, with cross-references between them.
3. **Code/notebooks.**  Optional: provide a Jupyter notebook verifying
   numerically the symmetric two-well ratio formula and the 2D
   quasipotential expansion (not required for theory paper acceptance,
   but strengthens reproducibility).
4. **Conference shortened version.**  After journal submission, prepare a
   10-page NeurIPS / ICML version focused on the closed-loop theorem and
   the testable predictions; keep the multivariate Freidlin–Wentzell
   section for the journal version only.

## What the user needs to fill in / check

- [ ] **Verify author affiliation.**  Paper currently lists "Independent
      Researcher".  If this changes (e.g.\ academic affiliation),
      update the `\author{...}` block.
- [ ] **Cross-reference paper-A and paper-B.**  The `\bibitem{paperA}` and
      `\bibitem{paperB}` entries currently use placeholder titles
      ("Companion paper, 2026").  Once paper-A and paper-B have arXiv IDs
      or DOIs, update with full citations.  **TODO**: paperA / paperB
      are TODO placeholders (no DOI / arXiv yet); will be filled in
      when companion papers are submitted.
- [ ] **Empirical validation (optional).**  The paper makes three testable
      predictions in §10.4.  Adding even small-scale empirical
      verification (e.g.\ on a synthetic two-well loss landscape) would
      strengthen the submission.  Not required but recommended.
- [ ] **Decide on JMLR-vs-Inf.Inf.**  If submitting to Information and
      Inference, the geometry option may need adjusting (Oxford journals
      have specific style requirements).  JMLR accepts the current
      `\documentclass[11pt,a4paper]{article}` style.
- [ ] **Section reorderings for conference version.**  A NeurIPS/ICML
      version would move the multivariate extension and the
      reparameterization invariance proof to the appendix.
- [ ] **Higher-order quasipotential terms.**  The 2D worked example is
      computed to order 4.  Carrying the expansion to order 6 (or
      providing a closed-form characterization of the full HJ solution)
      would address open question 1 in §10.3 and significantly raise
      the contribution; this is optional but high-value.
- [ ] **Numerical $H$ and $\Sigma$ measurements.**  For the testable
      prediction §10.4 (1), it would help to recommend a specific
      estimator for $\Sigma$ at a minimum (e.g.\ via mini-batch gradient
      sample variance).  Could be added as a remark.

## Compilation

```bash
cd /root/proof/sgd-convergence-theory/paper-c
pdflatex paper.tex && pdflatex paper.tex   # two passes for cleveref
```

Produces a 14-page PDF (`paper.pdf`).  Compiles cleanly with no errors.

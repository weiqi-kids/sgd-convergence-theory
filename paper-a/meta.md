# Paper A — Discrete Lyapunov Stability of SGD in the Interpolation Regime

## Summary

Per-sample Lyapunov exponent analysis for SGD at interpolating solutions under Jacobian orthogonality. Establishes a closed loop: discrete stability filtering → Jacobian complexity control → Rademacher generalization bound. Includes PAC-Bayes extension.

## Key Result

**Main Theorem (Theorem 5):** Under Assumptions A1–A3, SGD with learning rate η repels interpolating solutions with h_max > 2/η (positive Lyapunov exponent), attracts those with h_max < 2/η (negative exponents), and the surviving solutions satisfy a generalization bound of O(ρ/√(ηn)).

## Target Journals (ranked)

1. **JMLR** — best fit for theory-heavy, self-contained style. Estimated acceptance: 40–50% after revision.
2. **NeurIPS / ICML** — competitive but possible if condensed to 9 pages. Estimated: 25–35%.
3. **TMLR** — good fallback. Estimated: 50–60%.

## MSC 2020 Classification

- 68T07 (Computational learning theory)
- 62L20 (Stochastic approximation)
- 60H10 (Stochastic ordinary differential equations)

## Submission Strategy

1. Post to **arXiv** first (cs.LG + stat.ML)
2. Collect community feedback (2–4 weeks)
3. Submit to JMLR or next NeurIPS/ICML deadline

## Must-Cite Related Work

- Wu & Su (ICML 2023): tr(H) ≤ 2/η — our work refines to per-sample h_max
- Wu, Wang & Su (2022): noise alignment, ||H||_F bound
- Dinh et al. (2017): sharpness not reparameterization invariant
- Chaudhari & Soatto (2017): SGD as variational inference
- Furstenberg & Kesten (1960): random matrix products

## Known Limitations (address before submission)

1. **Assumption A3 (orthogonality)** is restrictive — discuss honestly, NTK whitening does NOT automatically satisfy it
2. **Linearized SGD only** — Theorem 5 Part (I) uses "linearized" qualifier; need to discuss nonlinear extension
3. **No global trajectory analysis** — results are local (near interpolating solutions)
4. **S⊥ behavior** — SGD drift on interpolation manifold not analyzed

## Reviewer's Most Likely Objection

"Assumption A3 trivializes the random matrix product into n independent scalar problems. The interesting case (non-commuting matrices) is left open."

Response strategy: Emphasize that (a) the per-sample perspective and closed-loop chain are the conceptual contributions, (b) exact Lyapunov exponents provide clear insight that approximate bounds do not, (c) state non-orthogonal extension as the primary open problem.

## File Info

- **Source:** paper.tex (1246 lines)
- **PDF:** paper.pdf (13 pages, 156 KB)
- **Compiled with:** tectonic, 0 errors

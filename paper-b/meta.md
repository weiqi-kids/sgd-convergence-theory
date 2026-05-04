# Paper B — When Does SGD Fail to Find Good Solutions?

## Summary

Four explicit counterexamples (F1–F4) delineating the failure boundaries of SGD's implicit regularization. F4 (noise-generalization reversal) is the main result: constructs a setting where the "good" solution has high gradient noise Σ and the "bad" solution has low Σ, causing SGD to systematically prefer the bad solution. Establishes four necessary conditions (N1–N4) for SGD to succeed.

## Key Result

**Theorem 1 (F4):** There exists a loss landscape where SGD converges to the bad solution with probability approaching 1, with escape time ratio τ_A/τ_B ~ exp(-19/η) → 0.

**Theorem 2:** Four conditions N1–N4 are each necessary for SGD's implicit regularization to succeed. Their conjunction is NOT proven sufficient.

## Target Journals (ranked)

1. **JMLR** — best fit for theoretical contribution. Estimated acceptance: 55–65% after revision.
2. **NeurIPS / ICML** — F4 alone could be a strong conference paper. Estimated: 40–50%.
3. **TMLR** — good fit. Estimated: 60–70%.

## MSC 2020 Classification

- 68T07 (Computational learning theory)
- 62L20 (Stochastic approximation)
- 60J20 (Markov chain applications)

## Submission Strategy

1. Post to **arXiv** first (cs.LG + stat.ML)
2. Collect feedback, especially on F4's practical relevance
3. Submit to JMLR or TMLR

## Must-Cite Related Work

- Wu & Su (ICML 2023): SGD stability conditions
- Li et al. (2021, "Shape Matters"): SGD prefers low Σ — our F4 shows when this hurts
- Chaudhari & Soatto (2017): SGD as variational inference
- Dinh et al. (2017): sharpness not invariant — our Prop 4 gives the invariant quantity
- Kramers (1940): escape rate theory
- Horsthemke & Lefever (1984): multiplicative noise SDE

## Known Limitations

1. **All constructions are 1D** — multi-dimensional extensions needed
2. **SDE approximation gap** — Kramers formula is for continuous-time; discrete SGD may differ
3. **F4 relies on posited data structure** — no concrete neural network example provided
4. **Necessary, not sufficient** — the conjunction N1∧N2∧N3∧N4 is not proven sufficient

## Reviewer's Most Likely Objection

"F4's construction is adversarial. How natural is the noise-generalization reversal in practice?"

Response strategy: Cite concrete settings (small datasets, shortcut features, distribution shift) where low-Σ solutions may memorize. Suggest empirical measurement of Corr(Σ, Gap) across benchmarks as future work.

## File Info

- **Source:** paper.tex (1034 lines)
- **PDF:** paper.pdf (12 pages, 122 KB)
- **Compiled with:** tectonic, 0 errors

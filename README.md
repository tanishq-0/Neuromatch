# Neuromatch: decoding the block prior from pre-stimulus activity

Can a mouse's learned expectation about which side the stimulus will appear on be read out from its frontal cortex *before* the stimulus arrives? Using public International Brain Laboratory (IBL) recordings, this notebook decodes the block prior, and the upcoming choice, from firing rates in the 0.5 s before stimulus onset.

This is my individual analysis strand of a six-person, three-week group project at Neuromatch Academy (Computational Neuroscience, July 2025). Credit to my project group, whose wider project this strand belongs to.

**View the notebook:** [on GitHub](silenthypo_2.ipynb) · [nbviewer](https://nbviewer.org/github/tanishq-0/Neuromatch/blob/main/silenthypo_2.ipynb) (if GitHub's preview doesn't load) · [open in Colab](https://colab.research.google.com/github/tanishq-0/Neuromatch/blob/main/silenthypo_2.ipynb)

## Key results

- **Data:** one IBL session, **586 trials**, **730 sorted units** on a probe insertion that passes through secondary motor cortex (MOs). Features are each unit's firing rate in the 0.5 s before stimulus onset.
- **Method:** logistic-regression decoders with **5-fold cross-validation** (± is the SD across folds), tested against **50 label permutations**.

| Decoding target | Trials | Accuracy | Largest class | Permutation test |
|---|---|---|---|---|
| Block prior, 3 classes (P(left) = 0.2 / 0.5 / 0.8) | 586 | **57.7% ± 10.6%** | 264 of 586 | 0 of 50 permutations as high |
| Block prior, strong-bias blocks only (0.2 vs 0.8) | 496 | **58.5% ± 4.2%** | 264 of 496 | 0 of 50 permutations as high |
| Choice, all contrasts | 586 | 51.5% ± 3.4% | 333 of 586 | p = 0.22 |
| Choice, zero-contrast trials | 74 | 59.4% ± 7.4% | 44 of 74 | p = 0.18 |

- **The block prior is decodable before the stimulus appears.** Both prior decoders were significant under label-permutation testing: none of the 50 shuffled-label decoders reached the real accuracy.
- **Choice was not decodable: a null result.** On zero-contrast trials, where there is no visual evidence and the mouse must rely on its prior, accuracy (59.4%) was no better than always guessing the more common choice (44 of 74 trials).

## How to run

The notebook was written for Google Colab; use the "Open in Colab" badge at the top of `silenthypo_2.ipynb`. To run it locally instead:

```bash
pip install ONE-api ibllib scikit-learn jupyter
jupyter notebook silenthypo_2.ipynb
```

Data are downloaded from IBL's public OpenAlyx server. `ONE(password='international')` is IBL's documented public, read-only login, not a private credential. The session's spike files are large (several hundred MB).

## Project structure

```
silenthypo_2.ipynb   data loading, pre-stimulus feature extraction, decoders, permutation tests
README.md
LICENSE
```

## Limitations

- One session and one probe. All sorted units on the probe were used, not only those anatomically in MOs, so the results need replicating across sessions and across frontal regions (e.g. PL, ACCd).
- With 50 permutations, the smallest p-value the test can show is coarse.
- The cross-validation folds were not shuffled, and label permutation breaks the task's block structure. Slow drifts in firing rate could therefore contribute to the prior decoding. A null that keeps the block structure, such as pseudo-sessions, would be a stricter test.
- The 730 features outnumber the trials in every analysis. Hyperparameters were left at their defaults, with no dimensionality reduction or nested cross-validation.

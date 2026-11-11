# Uncertainty Quantification in Deep Neural Networks

Master's thesis, Department of Computer Science, California State University, Long Beach, May 2024.
Carol Eunice Gudumotu. Committee: Arash Sarshar, Ph.D. (chair), Amin Rezaei, Ph.D., Ju Cheol Moon, Ph.D.

A regression model is only trustworthy if it can say how wrong it might be. This thesis compares how
five neural-network approaches quantify the uncertainty of their own predictions: the noise in the
data itself (aleatoric uncertainty) and the model's lack of knowledge, which should grow away from the
training data (epistemic uncertainty).

> **Correction (2026).** The thesis's headline result, that the deep operator network places 89% of
> test points inside its 95% band, far ahead of the other models, comes from an error in how that
> model's total predictive standard deviation was computed. The original files below are kept
> unchanged; corrected notebooks are in [`corrigendum/`](corrigendum/), and the erratum is maintained
> in [ERRATA.md](https://github.com/cgudumotu/uq/blob/main/ERRATA.md).

## What was done

- **Data.** A 1-D benchmark, y = sin(x/2) plus noise whose spread changes with x: 300 training points
  on [-7, 7] and 200 test points on [-10, 10], so part of the test set lies outside the training range.
  A three-input extension uses the Ishigami function.
- **Models.** A simple network with a mean and a variance output, a deep operator network (branch and
  trunk, with a random input to the trunk), a deep ensemble of five simple networks, a Bayesian neural
  network, and Monte Carlo dropout. Each network has about 3,900 parameters.
- **Evaluation.** Each model was trained and tested 10 times. Reported: the share of test points
  inside the model's "95%" band (coverage), inside and outside the training range, and the test error.

## What it shows, after correction

- With sigma computed the same, correct way for every model (the law of total variance), the deep
  operator network, the simple network and the deep ensemble perform comparably. MC dropout and the
  Bayesian network, which model only epistemic uncertainty, cover less.
- Networks trained with a Gaussian likelihood learn input-dependent noise well inside the training
  range: the aleatoric part works.
- Outside the training range no model reaches its stated coverage, and the deep operator network's
  epistemic signal is negligible.
- The test data are noisier than the training data, which caps every model's coverage on that test
  set. A fair comparison draws both the same way.
- The Ishigami results (Table 34) cannot be reproduced from the surviving notebooks.

## Results

Corrected values come from re-running the thesis notebooks' code with fixed seeds: 10 runs per model,
sigma computed by the law of total variance, the band still mu ± 2 sigma, on the same test set as the
thesis. They are under review with the erratum.

### Coverage of the "95%" band (Table 32)

Share of test points inside each model's band, in percent, published → corrected:

| Model | Full test set | Inside training range | Outside training range |
|---|---|---|---|
| Simple network | 63.45 → 60.9 | 65.92 → 67.5 | 47.99 → 52.8 |
| Deep operator network | 89.05 → 64.5 | 85.42 → 67.4 | 97.0 → 62.5 |
| Bayesian network | 13.3 → 15.0 | 8.57 → 8.1 | 21.16 → 24.7 |
| MC dropout | 52.15 → 50.1 | 41.35 → 42.1 | 72.49 → 67.0 |
| Deep ensemble | 59.7 → 62.5 | 36.21 → 68.2 | 88.16 → 57.3 |

Only the deep operator network and the deep ensemble had sigma errors. The other three rows change by
run-to-run variation, within 3 standard errors of the published values. Run-to-run standard
deviation: 2–5 points on the full test set, 1–3 inside the training range, 5–10 outside it.

### Test error (Table 33)

Mean squared error on the full test set, published → corrected:

| Model | MSE |
|---|---|
| Simple network | 0.147 → 0.151 |
| Deep operator network | 0.143 → 0.145 |
| Bayesian network | 0.352 → 0.147 |
| MC dropout | 0.198 → 0.169 |
| Deep ensemble | 0.154 → 0.151 |

The corrected column is the error of each model's averaged prediction, computed the same way for every
model. The published Bayesian value is that notebook's test loss, which includes a model-complexity
penalty. All five models are close to the noise floor of the test data, about 0.135 (the error of the
true curve itself).

### Ishigami function (Table 34)

Published: 76.18% of test points inside the band (95.65% inside the training range, 39.74% outside),
over 30 runs. These numbers cannot be reproduced: the notebook that produced them was not found, and
the surviving notebooks use a different setup. Corrected copies of four surviving notebooks are in
`corrigendum/` and will be re-run.

## Repository

| Path | Contents |
|---|---|
| `UQ_DNN_Thesis.pdf` | The thesis (May 2024) |
| `DNN_Thesis_files/` | The five notebooks behind the 1-D results (Tables 1-33), unchanged |
| `Ishigami_thesis_files/` | The surviving Ishigami notebooks, unchanged |
| `corrigendum/` | Corrected copies and their outputs ([README](corrigendum/README.md)) |

Related: [uqcalibrate](https://github.com/cgudumotu/uq), an open-source Python package for checking
whether a regression model's uncertainty is calibrated, which grew out of this work.

## Citation

Gudumotu, C. E. (2024). *Uncertainty Quantification in Deep Neural Networks* [Master's thesis,
California State University, Long Beach].

Licensed under the MIT License (see [LICENSE](LICENSE)).

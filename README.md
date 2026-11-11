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

Corrected numbers will be added here once the erratum is final.

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

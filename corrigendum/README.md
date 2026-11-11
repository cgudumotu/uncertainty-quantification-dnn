# Corrigendum

Corrected copies of thesis notebooks. Each one sits at the same path as its original, under
`corrigendum/`: `corrigendum/DNN_Thesis_files/Refactor_donn.ipynb` is the corrected copy of
`DNN_Thesis_files/Refactor_donn.ipynb`. The originals are unchanged (commit 2b6eb66).

## DNN_Thesis_files/Refactor_donn.ipynb

What changed from the original:

1. **Predictive standard deviation**, in `plot_with_confidence_interval`. The original added the
   square root of the standard deviation of the 50 sampled means to the square root of the mean
   predicted variance:

   ```python
   stds = std(mus) ** (1/2) + mean(vars) ** (1/2)
   ```

   The corrected line adds the two variances and takes one square root (law of total variance):

   ```python
   stds = sqrt(var(mus) + mean(vars))
   ```

   The band is still mu ± 2 * sigma.
2. **Last cell.** In the original, the assignment `tr_loss, ..., percentage_count_within_ci =` is
   split across two lines, which Python rejects with a `SyntaxError`. The two lines are joined so
   the cell runs.
3. **Setup.** Two cells added near the top (a `pip install` of the packages, and a printout of the
   kernel's Python and its installed packages), plus `import sys` and `print(sys.executable)` at
   the top of the imports cell.

No other code changed.

**A note on `var`.** `var(mus)` is torch's default `.var()`, which divides by n - 1 (n = 50 samples).
The erratum's corrected sigma, like `uqcalibrate.total_std`, divides by n. With the small spread of
the means here (about 0.011) the difference is negligible. The code is kept exactly as it was run,
so it matches the outputs below.

## Outputs of the run on 10 November 2026

Written by the last cell, once for each test set: `em` is the full test set, `em_idd` the points
inside the training range, `em_oodd` the points outside it. Each set has 10 runs.

| File | Contents |
|---|---|
| `donn_results_<set>.txt` | per run: training and testing loss, training and testing mean squared error, and the % of test points inside the band |
| `donn_summary_<set>.txt` | the mean and standard deviation of those columns over the 10 runs |
| `donn_arch_<set>.png` | a box plot of the losses and errors over the 10 runs |

The data are drawn with a fixed seed, but training is not seeded (there is no `torch.manual_seed`),
so a re-run gives slightly different numbers.

## Ishigami_thesis_files

Four corrected copies. Only the predictive standard deviation in `plot_with_confidence_interval`
changed. The copies hold no outputs yet: the saved outputs came from the old formula, so they were
cleared, and each notebook is re-run to fill them in.

| Notebook | Original sigma | Corrected sigma |
|---|---|---|
| `ishigami_Refactor_denn_x1.ipynb`, `_x2`, `_x3` (deep ensemble of 5, one input each) | `std(mus) ** (1/2)`: the square root of the spread of the member means; the members' predicted noise variances were computed but not used | `sqrt(var(mus) + mean(vars))` |
| `Ishigami_refactor_donn_x2_old.ipynb` (deep operator network, input x2) | `std(mus) ** (1/2) + mean(vars) ** (1/2)`, as in the DNN notebook above | `sqrt(var(mus) + mean(vars))` |

In these copies `var` divides by n (`correction=0`), as the erratum's corrected sigma does. The band
is still mu ± 2 * sigma. `Ishigami_refactor_donn_x2_old.ipynb` stands for three identical files in
the originals: `_x2_old`, `_x2_old(1)` and `_x2_old(2)` have the same SHA-256.

Not changed:

- `ishigami_Refactor_snn_x1.ipynb`: sigma is the square root of the predicted variance, already right.
- `ishigami_Refactor_bnn_x3.ipynb`, `ishigami_Refactor_mcdnn_x1.ipynb`: these models have no noise
  output; sigma is the spread of the sampled predictions, as designed.
- `Ishigami_refactor_donn_x1.ipynb`, `_x2`, `_x3` (three inputs): not corrected yet. Besides the
  sigma line, the inputs are built with an extra axis, (n, 3) to (n, 1, 3), so the branch and trunk
  outputs broadcast to an n × n grid: every input is paired with every noise draw, the loss compares
  every input with every target, and the prediction step averages over the inputs. Correcting sigma
  alone would not give a band per test point.

How these notebooks relate to Table 34 is covered in the erratum (E-5).

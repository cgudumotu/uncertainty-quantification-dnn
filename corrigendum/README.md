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

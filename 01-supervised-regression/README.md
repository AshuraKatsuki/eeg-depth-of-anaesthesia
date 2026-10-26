# Assignment 1: Supervised Regression of BIS

Part of [EEG Depth of Anaesthesia](../README.md). The goal is to build a new depth of anaesthesia
(DoA) index from six EEG-derived features and compare it with the Bispectral Index (BIS).

## Data

- 8 training cases and 4 test cases; each sample has six EEG features (`x1`–`x6`) and the BIS value.
- The dataset is not included (see the main README).

## Methods

1. Exploratory analysis: descriptive statistics, boxplots, feature–BIS scatter plots and a correlation
   heatmap. `x3` has the strongest correlation with BIS (r = 0.69), and `x2` and `x5` are strongly
   correlated with each other (r = 0.85).
2. Two models: Multiple Linear Regression and Random Forest Regressor.
3. Evaluation on the four test cases with Pearson correlation, Bland–Altman analysis and
   time-series comparison against BIS.

## Results

Mean across the four test cases:

| Model | Pearson r | R² | RMSE |
| --- | --- | --- | --- |
| Linear Regression | 0.8522 | 0.7046 | 8.0832 |
| Random Forest | 0.8048 | 0.5390 | 9.4486 |

Per test case:

| Test case | Model | Pearson r | R² | RMSE |
| --- | --- | --- | --- | --- |
| Test-1 | Linear Regression | 0.8889 | 0.7306 | 8.0636 |
| Test-1 | Random Forest | 0.8698 | 0.5154 | 10.8153 |
| Test-2 | Linear Regression | 0.9341 | 0.8621 | 7.8009 |
| Test-2 | Random Forest | 0.9387 | 0.8725 | 7.5014 |
| Test-3 | Linear Regression | 0.7270 | 0.4988 | 6.2280 |
| Test-3 | Random Forest | 0.5816 | 0.0878 | 8.4025 |
| Test-4 | Linear Regression | 0.8589 | 0.7269 | 10.2405 |
| Test-4 | Random Forest | 0.8292 | 0.6805 | 11.0754 |

Linear Regression was more consistent across cases; Random Forest dropped sharply on Test-3.

Bland–Altman limits of agreement on the test cases: Linear Regression −14.99 to 15.63
(mean difference 0.32); Random Forest −16.83 to 19.32 (mean difference 1.25).

## Figures

| File | Content |
| --- | --- |
| `figures/fig_heatmap.png` | Correlation heatmap of features and BIS |
| `figures/fig_boxplot.png` | Distribution of features and BIS |
| `figures/fig_scatter_features.png` | BIS against each feature |
| `figures/fig_scatter_pred_vs_bis.png` | Predicted index against BIS |
| `figures/fig_bland_altman.png` | Bland–Altman plots for both models |
| `figures/test_cases_comparison.png` | Predicted index and BIS over time for each test case |

## Limitations

- The random train/validation split leaks information, because neighbouring EEG windows from the same
  patient are highly similar. Validation scores are therefore optimistic.
- Model settings were partly chosen using the test cases, which also biases the results.
- Both issues are fixed in [Assignment 3](../03-combined-doa-index/).

## Files

- `final.ipynb`: full analysis notebook (requires the dataset)
- `figures/`: plots used in the report

# Assignment 3: A Combined Depth of Anaesthesia Index

Part of [EEG Depth of Anaesthesia](../README.md). The goal is to design one depth of anaesthesia (DoA)
index that combines at least two machine learning methods, and to compare it with the Bispectral
Index (BIS) and with the two single-model indices from [Assignment 1](../01-supervised-regression/).

## Data

- Same data as Assignment 1: six EEG-derived features (`x1`–`x6`) with BIS as the reference.
- Training: 8 cases, 20,184 samples. Test: 4 cases, 14,735 samples.
- The dataset is not included (see the main README).

## What changed from Assignment 1

Assignment 1 had three evaluation problems. This assignment fixes each one:

| Problem in Assignment 1 | Fix in Assignment 3 |
| --- | --- |
| Random split leaked information between neighbouring windows of the same patient | Case-wise cross-validation with `GroupKFold` (each fold holds out whole patients) |
| Model settings partly chosen on the test cases | All tuning and feature selection done inside training-only cross-validation |
| Feature selection mixed with model building | Feature selection and model building run as two separate steps |

The test cases were used **once**, after every choice had been fixed.

## Methods

1. **Feature selection:** all 63 non-empty subsets of the six features were scored with case-wise
   cross-validation for each base model. Random Forest and KNN selected {`x3`, `x5`, `x6`}; Linear
   Regression selected {`x1`, `x3`, `x5`, `x6`}. Because `x2` and `x5` are highly correlated
   (r = 0.85) and `x5` relates slightly more to BIS, `x2` was the one to drop. This reverses the
   choice made in Assignment 1.
2. **Base models:** Random Forest (`max_depth = 8`), KNN (`k = 200`, standardised inputs) and Linear
   Regression, tuned with case-wise cross-validation.
3. **Combined index (stacking):** a Ridge meta-model learns how to weight the three base models from
   their out-of-fold predictions, so it never sees predictions on data a base model was trained on.
4. **Post-processing:** the index is clipped to [0, 100] and smoothed over time (moving window of 91
   samples).
5. **Comparison:** stacking was compared with a simple average and a residual-correction variant.

Learned stacking weights:

| Base model | Weight |
| --- | --- |
| KNN | 0.6050 |
| Linear Regression | 0.2695 |
| Random Forest | 0.1320 |
| Intercept | −0.4203 |

## Results

Pearson correlation with BIS on the four unseen test cases:

| Test case | Pearson r |
| --- | --- |
| Test-1 | 0.8728 |
| Test-2 | 0.9540 |
| Test-3 | 0.7835 |
| Test-4 | 0.8948 |
| **Mean** | **0.8763** |

Adding short-term temporal context (a 31-sample window) raised the mean to **0.8869**.

Under the same leakage-free evaluation, the Assignment 1 models reach 0.8471 (Linear Regression) and
0.6881 (Random Forest). These differ from the numbers reported in Assignment 1 because that version
used a leaky random split.

## Limitations

- Performance varies between patients: Test-3 is clearly the hardest case (r = 0.78).
- BIS is itself a proprietary index, so the model learns to imitate BIS rather than true anaesthetic
  depth.
- Eight training patients is a small sample; results may not generalise to other patients or drugs.

## Files

- `notebook.ipynb`: full pipeline (requires the dataset)
- `figures/`: plots used in the report

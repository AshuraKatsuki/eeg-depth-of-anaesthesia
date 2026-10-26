# Depth of Anaesthesia Index from EEG Features

Coursework projects for CSC3250 Pattern Recognition at the University of Southern Queensland (2026).
Across three assignments, I build and evaluate machine learning models that estimate depth of
anaesthesia (DoA) from EEG-derived features, using the Bispectral Index (BIS) as the reference.

## Projects

| Folder | Task | Methods | Main result |
| --- | --- | --- | --- |
| [`01-supervised-regression`](01-supervised-regression/) | Predict BIS from six EEG features | Multiple Linear Regression, Random Forest | Mean Pearson r = 0.852 (Linear Regression) |
| [`02-unsupervised-clustering`](02-unsupervised-clustering/) | Separate awake and deep anaesthesia states without labels | K-Means, hierarchical clustering | Accuracy 0.963 (Ward) with feature sets {p1, p6, p7} |
| [`03-combined-doa-index`](03-combined-doa-index/) | Design one combined DoA index | Stacked ensemble (Random Forest + KNN + Linear Regression), GroupKFold | Mean Pearson r = 0.876 on 4 unseen test cases |

## How the work developed

- **Assignment 1** set up the regression baseline. Its random train/validation split leaked
  information between neighbouring EEG windows, which made validation scores optimistic.
- **Assignment 2** used unlabelled EEG segments and showed that a small feature subset carries most
  of the signal for separating anaesthesia states.
- **Assignment 3** fixed the evaluation problems from Assignment 1: case-wise cross-validation,
  feature selection on training data only, and a single run on the test cases.

## Data

The datasets are clinical data provided for the unit and are **not included** in this repository.
The notebooks need the original files to run.

## Requirements

Python 3, pandas, numpy, scikit-learn, scipy, matplotlib, seaborn, openpyxl

## Author

Tran Minh Anh Nguyen, Bachelor of Information Technology (AI and Data Science), UniSQ

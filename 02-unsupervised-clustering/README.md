# Assignment 2: Unsupervised Clustering of Anaesthesia States

Part of [EEG Depth of Anaesthesia](../README.md). The goal is to split 4,950 unlabelled EEG segments
into two groups (A and B) with two unsupervised methods, find which feature sets are most useful, and
compare the groups with the reference labels (awake / deep anaesthesia).

## Data

- 4,950 EEG segments, each described by seven feature sets (`p1`–`p7`), with no labels.
- A separate file gives reference labels (about 50% awake, 50% deep anaesthesia), used for evaluation only.
- The datasets are not included (see the main README).

## Methods

1. **Data analysis:** descriptive statistics, boxplots and a correlation heatmap. Neighbouring
   feature sets are strongly correlated (`p3`–`p4` r = 0.90, `p6`–`p7` r = 0.91), while the two ends of
   the range move in opposite directions (`p1`–`p7` r = −0.20).
2. **Preprocessing:** all features standardised (z-scores), because both methods rely on Euclidean
   distance and a feature with larger variance would otherwise dominate.
3. **Clustering:** K-Means (k = 2) and agglomerative hierarchical clustering with Ward linkage.
4. **Feature usefulness:** K-Means on each feature set alone, scored by silhouette and by accuracy
   against the reference labels.
5. **Evaluation:** silhouette score, accuracy and confusion matrix against the reference labels,
   agreement and Adjusted Rand Index (ARI) between the two methods, and a PCA check.

## Results

**All seven feature sets**

| Method | Cluster sizes (A / B) | Silhouette | Accuracy |
| --- | --- | --- | --- |
| K-Means | 927 / 4023 | 0.355 | 0.511 |
| Hierarchical (Ward) | 488 / 4462 | 0.392 | 0.569 |

The two methods agreed on 91.1% of segments (ARI = 0.568), yet both were close to chance against the
reference labels. They found the same structure, but it was not the awake / deep split.

**Usefulness of each feature set (K-Means on one feature)**

| Feature set | Silhouette | Accuracy |
| --- | --- | --- |
| p6 | 0.569 | 0.849 |
| p7 | 0.658 | 0.791 |
| p1 | 0.558 | 0.770 |
| p2 | 0.562 | 0.670 |
| p5 | 0.490 | 0.542 |
| p4 | 0.521 | 0.516 |
| p3 | 0.574 | 0.514 |

**Selected feature sets {p1, p6, p7}**

| Method | Silhouette | Accuracy |
| --- | --- | --- |
| K-Means | 0.443 | 0.915 |
| Hierarchical (Ward) | 0.435 | 0.963 |

Hierarchical clustering on {p1, p6, p7} matched 2,358 of 2,484 awake segments and 2,410 of 2,466
deep anaesthesia segments.

**PCA:** PC1 and PC2 explain 85.3% of the variance (0.516 + 0.336), but K-Means on the first one to
three components stayed at 0.511–0.516 accuracy. The direction of greatest variance is not the
direction that separates the two states.

## Key findings

- Choosing informative feature sets mattered far more than using all of them: accuracy rose from about
  0.5 to 0.96.
- Agreement between two methods does not mean they are correct; both can capture the same irrelevant
  structure.
- Ward linkage handled the selected features better than K-Means, which assumes spherical clusters of
  similar size.

## Limitations

- Feature sets were selected using the reference labels, so the 0.963 accuracy is optimistic for truly
  unlabelled data.
- Only two methods from the unit were allowed; a Gaussian mixture model could handle elliptical
  clusters.

## Figures

| File | Content |
| --- | --- |
| `figures/boxplot.png` | Distribution of the seven feature sets |
| `figures/heatmap.png` | Correlation between feature sets |
| `figures/scatter.png` | `p6` vs `p7` and `p1` vs `p7` |
| `figures/dendrogram.png` | Ward dendrogram on all seven feature sets |
| `figures/pca_scatter.png` | PC1–PC2 coloured by K-Means cluster and by reference label |

## Files

- `notebook.ipynb`: full analysis (requires the datasets)
- `figures/`: plots used in the report

# Predicting Gene Expression from Chromatin Accessibility (PBMC Multiome)

Can the "openness" of DNA near a gene predict how much that gene is expressed?
This project answers that question using paired single-cell **ATAC-seq** (chromatin accessibility) and **RNA-seq** (gene expression) data from human peripheral blood mononuclear cells (PBMCs), modeled with **ridge regression**.

**Authors:** Geethanjali Karuturi, Fredrick Onyango

---

## Key Results

| Metric | Value |
|---|---|
| Genes modeled | 1,000 |
| Median R² | 0.607 |
| Median Pearson r | 0.892 |
| Genes with positive R² | 80.1% |
| Genes with R² > 0.5 | 57.9% |
| Genes beating the permutation null (95th percentile) | 84.3% |
| Median permutation R² (random baseline) | −0.194 |

**Example gene – RNF13:** held-out test R² ≈ 0.89, Pearson r ≈ 0.97, using ~13 nearby ATAC peaks.

Top predicted genes included immune-related genes such as **LYN, MS4A6A, ARHGAP24, RBM47, DOCK5** (R² close to 1.0).

> Note: the mean R² (0.152) is much lower than the median because a small number of genes had strongly negative R², meaning local accessibility alone could not explain their expression.

---

## Workflow

```
10x PBMC Multiome (~10,000 cells)
        ↓
Quality control (7,537 genes, 111,743 ATAC peaks kept)
        ↓
TF-IDF normalization (ATAC) + Leiden clustering
        ↓
Pseudobulk: combine cells within each cluster (35 clusters)
        ↓
For each gene: take ATAC peaks within ±100 kb of its TSS
        ↓
Ridge regression (alpha tuned by 5-fold cross-validation)
        ↓
Permutation test (100 shuffles per gene)
        ↓
Gene-level performance (R², Pearson r)
```

**Why ridge regression?** Nearby ATAC peaks are often highly correlated with each other. Ridge regression adds an L2 penalty that keeps the model stable when predictors overlap.

**Why pseudobulk?** Single-cell data is very sparse and noisy. Averaging cells within each cluster reduces noise while keeping cell-type differences.

---

## Repository Structure

```
pbmc-ridge-regression/
├── notebooks/
│   └── ridge_regression_analysis.ipynb   # full analysis
├── requirements.txt                      # Python packages
└── README.md
```

---

## How to Run

1. Download these two files and put them in a folder called `data/` (they are not stored in this repo because of their size):
   - `10k_PBMC_Multiome_nextgem_Chromium_X_filtered_feature_bc_matrix.h5` (10x Genomics public datasets)
   - `gencode.v38.annotation.gtf.gz` (GENCODE human release 38)
2. Install packages:
   ```bash
   pip install -r requirements.txt
   ```
3. Open `notebooks/ridge_regression_analysis.ipynb` in Jupyter and run all cells.

---

## Tools

Python · Scanpy · scikit-learn · PyRanges · NumPy · Pandas · Matplotlib · Seaborn

---

## Limitations

- Ridge regression is linear, so it cannot capture non-linear regulation.
- Only peaks within ±100 kb were used; distal enhancers further away are missed.
- Pseudobulking hides cell-to-cell variation within clusters.

## Future Work

- Add transcription factor motif analysis
- Use chromatin interaction (Hi-C) data to include distal enhancers
- Compare with non-linear models (e.g., random forest, gradient boosting)

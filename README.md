# pbmc-ridge-regression
Goal

The goal of this project is to study cis-regulatory chromatin-expression relationships and evaluate how effectively local chromatin accessibility predicts gene expression in immune-cell populations.

RNA expression prediction using ATAC accessibility and ridge regression

This project uses paired single-cell RNA-seq and ATAC-seq PBMC multiome data to predict gene expression from nearby chromatin accessibility peaks using ridge regression.

Methods
TF-IDF normalization
Leiden clustering
Pseudobulking
Ridge regression
5-fold cross-validation
Permutation testing
Dataset
10x Genomics PBMC Multiome dataset
~10,000 cells
~11,000 genes

100,000 ATAC peaks

Results
1000 genes modeled
80.1% genes achieved positive R²
57.9% genes achieved R² > 0.5
84.3% genes exceeded permutation null threshold

RNF13 was analyzed as a representative high-performing gene with:

Test R² ≈ 0.89
Pearson r ≈ 0.97

Tools Used
Python
Scanpy
Scikit-learn
NumPy
Pandas
Matplotlib

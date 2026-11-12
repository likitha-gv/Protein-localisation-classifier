# Yeast Protein Localisation Predictor
### A machine learning classification pipeline for predicting subcellular compartments from protein sequence features

## Overview
A protein's location in the cell determines its function, but measuring that location experimentally is slow and costly. This project trains and compares supervised classifiers that predict which of ten subcellular compartments a budding yeast (*Saccharomyces cerevisiae*) protein belongs to, using eight sequence-derived features. The data are 1,464 labelled proteins.

## Key Features
- **Exploratory analysis**: class imbalance, per-class feature distributions, and Pearson/Spearman correlation checks for redundant features.
- **Leak-free pipeline**: stratified 80/20 train/test split, with `MinMaxScaler` and the classifier wrapped in a single scikit-learn `Pipeline`.
- **Hyperparameter tuning**: `GridSearchCV` with 5-fold stratified cross-validation, optimised for **macro-F1** so rare compartments count as much as common ones.
- **Five models compared**: Logistic Regression, Support Vector Machine (SVC), Random Forest, Gradient Boosted Trees and a Multilayer Perceptron.
- **Interpretability**: Gini and permutation feature importance for the selected model.
- **Inference**: Predictions with confidence scores for 20 unlabelled proteins.

**Tech stack:** Python, scikit-learn, pandas, NumPy, Matplotlib, seaborn, Jupyter

## Key Results

Test-set performance (held-out 20%, 293 proteins):

| Model | Accuracy | F1 (Weighted) | F1 (Macro) |
|---|---|---|---|
| **Random Forest** (selected) | **0.594** | **0.583** | **0.595** |
| SVC | 0.573 | 0.567 | 0.595 |
| Gradient Boosted Trees | 0.614 | 0.602 | 0.583 |
| Logistic Regression | 0.573 | 0.571 | 0.531 |
| Multilayer Perceptron | 0.580 | 0.567 | 0.492 |

- **Random Forest** was selected for the following reasons:
  - It tied for the best macro-F1 and had the highest cross-validation macro-F1 (0.590).
  - It stayed competitive on accuracy and weighted-F1.
- The signal-peptide (`mcg`) and transmembrane (`alm`) scores are the most important features.
- The main remaining source of error is separating cytosolic (CYT) from nuclear (NUC) proteins, whose feature profiles largely overlap.

## Project Structure
```
├── data/
│   ├── yeast-labelled.csv      # 1,464 labelled proteins (8 features + site)
│   └── yeast-unlabelled.csv    # 20 proteins to predict
├── notebooks/
│   └── yeast_classification_project.ipynb
├── README.md
└── requirements.txt
```
## Data
Yeast protein localisation data with eight features: `mcg`, `gvh` (signal-peptide scores), `alm` (transmembrane), `mit` (mitochondrial), `erl` (ER retention), `pox` (peroxisomal), `vac` (vacuolar) and `nuc` (nuclear). The target is one of ten compartments: CYT, NUC, MIT, ME1, ME2, ME3, EXC, VAC, POX and ERL. 

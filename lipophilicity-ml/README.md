# Predicting Molecular Lipophilicity with Machine Learning

An exploratory machine learning project comparing molecular descriptors and Morgan fingerprints for predicting experimental lipophilicity. The notebook evaluates linear regression, ridge regression, decision trees, random forests, gradient boosting and support vector regression.

## Data

The `data/lipophilicity_astrazeneca.tab` file contains 4,200 compounds with `Drug_ID` (ChEMBL identifier), `Drug` (SMILES) and `Y` (experimental lipophilicity). The endpoint is the octanol/water distribution coefficient **logD at pH 7.4**, not the computed descriptor LogP. This is the AstraZeneca lipophilicity dataset included in MoleculeNet. The data originates from the ChEMBL deposited AstraZeneca dataset and was distributed through MoleculeNet/TDC. See [MoleculeNet](https://pmc.ncbi.nlm.nih.gov/articles/PMC5868307/) and [TDC](https://tdcommons.ai/single_pred_tasks/adme/). The data is attributed to its original providers; this repository does not claim ownership of it.

## Run locally

1. Create a Python 3.11 environment and install dependencies:

   ```bash
   python -m venv .venv
   source .venv/bin/activate  # Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```

2. From the project root, launch Jupyter: `jupyter notebook lipophilicity_prediction.ipynb`.
3. Run cells in order. The grid search and fingerprint models may take several minutes.

## What the notebook does

- Inspects target distribution and missing values.
- Computes RDKit molecular weight, calculated LogP, aromatic rings, hydrogen bond donors and acceptors.
- Compares six regressors using an 80/20 random split (`random_state=42`).
- Tunes a random forest on descriptor features with five-fold cross-validation.
- Repeats the model comparison with 2,048-bit radius-2 Morgan fingerprints.

In the saved exploratory run, the best fingerprint model (SVR) has RMSE **0.774** and R² **0.594** on the random holdout. The tuned descriptor random forest has RMSE **0.970** and R² **0.363**. Saved outputs are from the original local run; dependencies and execution have not been revalidated in this package.

## Limitations

**The test set is used to compare and choose models.** The quoted scores are therefore exploratory and may overstate the performance of the selected model. A stronger evaluation would choose the representation and hyperparameters using cross-validation within the training set and evaluate the final model once on a held-out set. Random splits can also place similar chemical structures in train and test; a scaffold split would test generalization to new chemical families more realistically. Invalid SMILES are assigned missing descriptors or zero fingerprints in the original notebook, rather than being excluded through a shared validation rule.

Project by Nisa Özkan. No license is granted for the project code at this stage; data rights remain with the original providers.

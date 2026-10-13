# Phishing Website Risk Assessment using Machine Learning

An end-to-end predictive analytics project that classifies phishing and legitimate websites using the **PhiUSIIL Phishing URL (Website)** dataset. The workflow follows the **Knowledge Discovery in Databases (KDD)** process and includes model evaluation, phishing-risk bands, and feature-importance analysis.

## Project objectives

- Explore and understand a labelled phishing website dataset.
- Prepare numeric engineered features for machine learning.
- Train a Random Forest classifier to distinguish phishing from legitimate examples.
- Evaluate the model using precision, recall, F1-score, ROC-AUC, PR-AUC, and a confusion matrix.
- Convert predicted phishing probabilities into Low, Medium, and High risk bands.
- Inspect feature importance to understand which inputs the fitted model relies on.

## Dataset

**PhiUSIIL Phishing URL (Website)** — UCI Machine Learning Repository, dataset ID 967.

- Dataset page: https://archive.ics.uci.edu/dataset/967/phiusiil+phishing+url+website
- Records in the downloaded CSV: **235,795**
- Columns including target: **56**
- Source label `0`: phishing — **100,945** records
- Source label `1`: legitimate — **134,850** records
- Initial inspection: **0 missing values** and **0 duplicate rows**

The notebook recodes the target for modelling so that **phishing = 1** and **legitimate = 0**. This mapping is important: the source dataset uses the opposite convention.

### Download and place the data

1. Download the dataset from the official UCI page above.
2. Extract the CSV if it is supplied in an archive.
3. Place the CSV beside the notebook and name it `PhiUSIIL_Phishing_URL_Dataset.csv`.

The dataset is not included in this repository. Please follow the dataset provider's terms when downloading or redistributing it.

## KDD workflow

1. **Selection:** load PhiUSIIL and inspect its shape, labels, missing values, duplicates, and class distribution.
2. **Preprocessing:** remove duplicate rows and safely convert numeric feature values.
3. **Transformation:** select numeric engineered features, map the target so phishing is the positive class, and create a stratified train/test split.
4. **Data mining:** train a Random Forest classifier using a scikit-learn pipeline with median imputation.
5. **Evaluation:** measure precision, recall, F1-score, ROC-AUC, PR-AUC, and inspect the confusion matrix.
6. **Interpretation:** summarize risk bands and review feature importance.

## Model

The notebook uses a Random Forest classifier with:

- 100 trees (`n_estimators=100`)
- Maximum depth of 15
- Balanced class weights
- Fixed random seed for reproducibility
- Median imputation within a scikit-learn pipeline
- Stratified 80:20 train/test split

## Observed results

The current notebook run reported the following test-set results:

| Metric                     | Observed result |
| -------------------------- | --------------: |
| Test records               |          47,159 |
| Accuracy                   |            100% |
| Precision — legitimate     |          1.0000 |
| Recall — legitimate        |          1.0000 |
| F1-score — legitimate      |          1.0000 |
| Precision — phishing       |          1.0000 |
| Recall — phishing          |          1.0000 |
| F1-score — phishing        |          1.0000 |
| ROC-AUC                    |          1.0000 |
| PR-AUC / Average Precision |          1.0000 |
| Misclassified test records |               0 |

The confusion matrix showed 26,970 legitimate and 20,189 phishing records correctly classified, with zero errors.

### Risk-band summary

| Risk band | Test records | Observed phishing rate |
| --------- | -----------: | ---------------------: |
| Low       |       26,970 |                     0% |
| Medium    |            4 |                   100% |
| High      |       20,185 |                   100% |

The Low/Medium/High thresholds are project-defined reporting categories, not validated security standards.

### Most important features

| Feature            | Random Forest importance |
| ------------------ | -----------------------: |
| URLSimilarityIndex |                   0.1952 |
| NoOfExternalRef    |                   0.1709 |
| LineOfCode         |                   0.1459 |
| NoOfSelfRef        |                   0.1132 |
| NoOfImage          |                   0.0884 |

Feature importance describes how the fitted model uses the available inputs; it does **not** establish causation.

> **Important validation caveat:** Perfect test scores are unusually high for a real-world phishing detection task. Investigate possible target leakage, dataset-specific signals, and related or near-duplicate websites across the train/test split before claiming generalization. These numbers are the observed results on this particular split, not proof that the model can detect every real-world phishing website.

## Installation and usage

Recommended: Python 3.10 or newer.

```bash
# Clone the repository
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd phishing-risk-assessment-ml

# Create and activate a virtual environment (Windows)
python -m venv .venv
.venv\\Scripts\\activate

# Install dependencies
pip install -r requirements.txt

# Start Jupyter
jupyter notebook
```

Open the notebook and run its cells from top to bottom. The notebook reads the local CSV file described in the Dataset section. If your CSV has a different filename or location, update the `pd.read_csv(...)` path accordingly.

## Repository structure

```text
phishing-risk-assessment-ml/
├── PhiUSIIL_KDD_Simple.ipynb
├── README.md
├── requirements.txt
├── .gitignore
└── PhiUSIIL_Phishing_URL_Dataset.csv   # local only; not committed
```

## Limitations and future work

- Validate results on an independent or time-separated dataset.
- Check for target leakage and related domains across data splits.
- Choose operational risk thresholds using validation data and the cost of false positives versus missed phishing.
- Build a safe feature-extraction process before using the model to score arbitrary live URLs.
- Monitor performance as website structures and phishing tactics change.

This is currently a **dataset-based predictive analytics experiment**, not a live URL scanner. It expects precomputed numeric features from the dataset.

## References

1. PhiUSIIL Phishing URL (Website), UCI Machine Learning Repository, dataset 967. https://archive.ics.uci.edu/dataset/967/phiusiil+phishing+url+website
2. Safi, A. and Singh, S. (2023). “A systematic literature review on phishing website detection techniques.” _Journal of King Saud University – Computer and Information Sciences_, 35(2), 590–611. https://doi.org/10.1016/j.jksuci.2023.01.004
3. Tang, L. and Mahmoud, Q. H. (2021). “A Survey of Machine Learning-Based Solutions for Phishing Website Detection.” _Machine Learning and Knowledge Extraction_, 3(3), 672–694. https://doi.org/10.3390/make3030034

## License

Add a license file if you intend to publish the code for reuse. Check the dataset provider's terms separately before redistributing dataset files.

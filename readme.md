# Mind-Metrics: Diabetes Risk Modeling (BRFSS 2015)

## Overview
Mind-Metrics analyzes the 2015 Behavioral Risk Factor Surveillance System (BRFSS) survey to understand how lifestyle, clinical, and demographic factors relate to diabetes prevalence. The project cleans and prepares the data, explores relationships through EDA and hypothesis testing, and validates findings with predictive modeling and scaling techniques for efficient training.

## Dataset
- **Source:** BRFSS 2015 public health survey.
- **Files used:**
  - `diabetes_012_health_indicators_BRFSS2015.csv` (multiclass: 0 = No diabetes, 1 = Prediabetes, 2 = Diabetes)
  - `diabetes_binary_health_indicators_BRFSS2015.csv` (binary: 0 = Non-diabetic, 1 = Diabetic/Prediabetic)
- **Shape:** ~253,680 rows × 22 columns (after cleaning and encoding).
- **Target:** `Diabetes_binary` (0/1).
- **Features:** BMI, HighBP, HighChol, PhysActivity, Fruits, Veggies, Smoker, GenHlth, Age group, Education, Income, and related health indicators.

## Repository Structure
- `Data Preprocessing.ipynb` — cleaning, encoding, balancing, and export of processed CSVs.
- `Exploratory Data Analysis.ipynb` — univariate, bivariate analysis, correlations, and visualizations.
- `Hypothesis Testing.ipynb` — statistical tests (Welch t-test, chi-square, Fisher, Mann–Whitney) for key factors.
- `Model_Training.ipynb` — baseline and experimental models (e.g., logistic regression with scaling).
- `Scaling_Technique.ipynb` — efficiency experiments (row subsampling, JL transform, coreset via MiniBatchKMeans).
- `Validation Methods.ipynb` — cross-validation and evaluation workflows.
- `Previous Analysis on a Smaller Dataset/` — earlier phase notebooks and artifacts.
- `required csv/` — processed datasets (`diabetes_binary_health_indicators_BRFSS2015.csv`, balanced splits `X_train_over.csv`, `X_train_under.csv`, etc.).

## Environment Setup
1. Use Python 3.9+.
2. Recommended packages: `pandas`, `numpy`, `scikit-learn`, `scipy`, `matplotlib`, `seaborn`, `xgboost`.
3. Install dependencies (example):
   ```bash
   pip install pandas numpy scikit-learn scipy matplotlib seaborn xgboost
   ```

## How to Reproduce
1. Open the notebooks in order:
   - `Data Preprocessing.ipynb`
   - `Exploratory Data Analysis.ipynb`
   - `Hypothesis Testing.ipynb`
   - `Model_Training.ipynb`
   - `Scaling_Technique.ipynb`
2. Ensure the `required csv/diabetes_binary_health_indicators_BRFSS2015.csv` file is present; the preprocessing notebook also generates the balanced train splits.
3. Run cells top-to-bottom to reproduce cleaning, analysis, and modeling.

## Preprocessing Summary
- Removed coded missing values (7, 9, 77, 88, 99) per BRFSS documentation.
- Renamed columns for readability and converted all features to numeric types.
- Combined prediabetes and diabetes into a single positive class for the binary label.
- Balanced training data via oversampling and undersampling (saved as `X_train_over.csv`, `y_train_over.csv`, `X_train_under.csv`, `y_train_under.csv`).
- Final cleaned dataset: no nulls; all numeric; 22 features.

## EDA Highlights
- Class distribution: ~84.2% non-diabetic, ~15.8% diabetic/prediabetic.
- Positive associations with diabetes: HighBP, HighChol, BMI, poorer `GenHlth`, older `Age` groups.
- Protective/negative associations: higher `PhysActivity`, `Education`, `Income`.
- Visualizations include histograms/boxplots for key numerics, countplots for categorical variables, and correlation heatmaps (Pearson and Spearman).

## Hypothesis Testing
- Welch’s t-test: BMI differs significantly between diabetic and non-diabetic groups (p < 0.0001).
- Chi-square/Fisher: significant associations for PhysActivity, Smoker, HighBP (p < 0.0001).
- Mann–Whitney U: GenHlth distributions differ significantly (p < 0.0001).
- Interpretation: worse general health, higher BMI, high BP, and low activity levels align with higher diabetes prevalence.

## Modeling Snapshot
- Baseline: Logistic Regression with StandardScaler on balanced training data.
- Representative metrics (binary classification):
  - Accuracy ≈ 0.73; ROC-AUC ≈ 0.82.
  - Recall for positive class ≈ 0.76; Precision ≈ 0.34; F1 ≈ 0.47.
- Feature importance (log-odds): GenHlth, BMI, Age, HighBP, HighChol are top positive contributors; Income shows a negative association.

## Scaling Techniques for Efficiency (XGBoost focus)
- **Base model:** XGBoost trained on full feature set and full training split.
- **Row subsampling:** Random sampling of training rows to reduce n; maintains stratification, lowers training time.
- **JL Transform:** Gaussian random projection to reduce dimensionality (e.g., 21 → 10 features) while approximately preserving pairwise distances.
- **Coreset (MiniBatchKMeans):** Clusters training data and selects one representative per cluster (~5% of rows). Uses MiniBatchKMeans for faster clustering and vectorized center-to-point matching.
- **Comparison plots:** Accuracy and training time charts contrast base vs. scaled variants in `Scaling_Technique.ipynb`.

## Key Findings
- Statistically and experimentally, BMI, GenHlth, Age, HighBP, and HighChol are strong predictors of diabetes risk.
- Class imbalance requires balancing for fair model training; oversampling improved recall without excessive precision loss.
- Approximate scaling methods (row subsampling, JL, coresets) reduce training time with minimal accuracy drop for XGBoost on this dataset.

## Assumptions and Limits
- BRFSS coded missing values are treated as invalid responses and removed.
- Ordinal encodings (Age, Education, GenHlth) preserve rank meaning.
- Prediabetes and diabetes are merged for the binary task to emphasize risk presence.
- Very large sample size drives extremely small p-values; effect sizes and practical significance should be considered alongside p-values.

## Next Steps
- Add calibration (e.g., Platt scaling) and threshold tuning to balance precision/recall for screening use-cases.
- Evaluate tree-based models (XGBoost, LightGBM) with SHAP for explainability on the cleaned dataset.
- Extend scaling experiments to include Randomized SVD/feature hashing and benchmark against JL/coreset.
- Package preprocessing and inference into a lightweight CLI or API for batch scoring.
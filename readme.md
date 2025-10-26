# Diabetes Prediction and Risk Factor Analysis (BRFSS 2015 Dataset)

## 1. Project Overview
This project analyzes the Behavioral Risk Factor Surveillance System (BRFSS) 2015 dataset to study the association between health indicators and diabetes prevalence. The objective is to clean and prepare the dataset, explore the relationships between lifestyle and medical factors, perform hypothesis testing to identify statistically significant associations, and validate those findings through experimental modeling.

---

## 2. Dataset Explanation
**Source:** BRFSS 2015 (Behavioral Risk Factor Surveillance System)

**File Used:**  
`2015.csv` → subset of selected columns saved as:
- `diabetes_012_health_indicators_BRFSS2015.csv` (multiclass: 0 = No diabetes, 1 = Prediabetes, 2 = Diabetes)
- `diabetes_binary_health_indicators_BRFSS2015.csv` (binary: 0 = Non-diabetic, 1 = Diabetic/Prediabetic)

**Final Shape:** 253,680 rows × 22 columns

**Features (Independent Variables):**
Health, lifestyle, and demographic indicators such as:
- BMI, HighBP, HighChol, PhysActivity, Fruits, Veggies, Smoker, GenHlth, Age, Education, Income, etc.

**Target Variable:**
- `Diabetes_binary` → 0 (Non-Diabetic) or 1 (Diabetic/Prediabetic)

---

## 3. Problem Statement
To determine how lifestyle, demographic, and health indicators influence the likelihood of diabetes.  
The goal is to identify key risk factors and validate these statistically and experimentally to build a foundation for predictive modeling in healthcare analytics.

**Importance:**
Understanding these relationships supports early detection, prevention, and resource allocation in public health, aligning with goals of personalized healthcare and chronic disease management.

---

## 4. Data Preprocessing

### 4.1 Steps Performed
1. **Column selection:** Extracted 22 relevant columns from the original BRFSS file.
2. **Invalid value removal:** Removed coded missing values (7, 9, 77, 88, 99) based on BRFSS documentation.
3. **Re-encoding:**
   - Converted categorical responses into binary or ordinal (0/1 or 1–8) values.
   - Combined prediabetes and diabetes into a single binary label.
4. **Feature renaming:** For readability (e.g., `_RFHYPE5` → `HighBP`).
5. **Handling missing data:** Dropped NA rows after cleaning (final dataset has no null values).
6. **Type conversion:** Ensured all columns are numeric (`float64`).

### 4.2 Files Generated
| File | Description |
|------|--------------|
| `diabetes_012_health_indicators_BRFSS2015.csv` | Multiclass (0/1/2) version |
| `diabetes_binary_health_indicators_BRFSS2015.csv` | Binary version (0/1) |
| `X_train_over.csv`, `y_train_over.csv` | Balanced training data (oversampled) |
| `X_train_under.csv`, `y_train_under.csv` | Balanced training data (undersampled) |

### 4.3 Effects of Preprocessing
- Removed noisy and missing entries → cleaner, smaller dataset.
- Numeric encoding → enabled correlation and model training.
- Balancing through oversampling → mitigated class imbalance (16% diabetics originally).

---

## 5. Exploratory Data Analysis (EDA)

### 5.1 Overview
- **Shape:** (253,680, 22)
- **Missing Values:** None
- **Data Types:** All numeric (`float64`)
- **Target Distribution:**
  - 0 (Non-diabetic): 84.2%
  - 1 (Diabetic/Prediabetic): 15.8%

### 5.2 Key EDA Techniques
1. **Summary Statistics:**
   - Mean BMI ≈ 28.3, Mean Age group ≈ 8 (≈45–49 years)
   - 43% have high blood pressure
   - 42% have high cholesterol
2. **Visual Analysis:**
   - Histograms and boxplots for BMI, Age, GenHlth.
   - Countplots for categorical features like PhysActivity, HighBP.
   - Heatmaps for Pearson and Spearman correlations.
3. **Correlation Insights:**
   - Positive correlations: `HighBP`, `HighChol`, `BMI`, `GenHlth`
   - Negative correlations: `PhysActivity`, `Education`, `Income`

**Conclusion:** Unhealthy lifestyle and poor general health are associated with higher diabetes prevalence.

---

## 6. Hypothesis Testing

### 6.1 Objective
To statistically validate whether key health and lifestyle features significantly differ between diabetic and non-diabetic individuals.

### 6.2 Tests Performed
| Test | Variables | Hypothesis | p-Value | Decision |
|------|------------|-------------|----------|-----------|
| Welch’s t-test | BMI vs Diabetes | Mean BMI differs | < 0.0001 | Significant |
| Chi-square | PhysActivity vs Diabetes | Association exists | < 0.0001 | Significant |
| Chi-square | Smoker vs Diabetes | Association exists | < 0.0001 | Significant |
| Fisher’s Exact | HighBP vs Diabetes | Odds differ | < 0.0001 | Significant |
| Mann–Whitney U | GenHlth vs Diabetes | Distribution differs | < 0.0001 | Significant |

### 6.3 Interpretation
- Diabetics have higher BMI and worse GenHlth scores.
- Those with high BP or low physical activity have significantly higher diabetes prevalence.
- All tests reject the null hypothesis (α = 0.05).

---

## 7. Experiments Conducted to Validate Hypothesis Tests

### 7.1 Experimental Setup
- **Training Data:** Oversampled balanced dataset (`X_train_over.csv`, `y_train_over.csv`)
- **Testing Data:** Real unbalanced test set split from cleaned data.
- **Model Used:** Logistic Regression with StandardScaler pipeline.
- **Goal:** To see if model identifies the same significant features as the hypothesis tests.

### 7.2 Model Results
| Metric | Value | Interpretation |
|---------|--------|----------------|
| Accuracy | 0.73 | 73% correct predictions |
| Precision (1) | 0.34 | Moderate specificity |
| Recall (1) | 0.76 | Captures most diabetics |
| F1-score (1) | 0.47 | Balanced performance |
| ROC-AUC | 0.82 | Good separability between classes |

**Confusion Matrix:**
| | Predicted 0 | Predicted 1 |
|--|-------------|-------------|
| Actual 0 | 46,384 | 17,727 |
| Actual 1 | 2,853 | 9,140 |

---

### 7.3 Feature Importance (Model Coefficients)
| Rank | Feature | Coefficient | Interpretation |
|------|----------|-------------|----------------|
| 1 | GenHlth | +0.61 | Poorer health → higher diabetes risk |
| 2 | BMI | +0.53 | Higher BMI → higher diabetes risk |
| 3 | Age | +0.44 | Older individuals more likely diabetic |
| 4 | HighBP | +0.33 | Strong association with diabetes |
| 5 | HighChol | +0.30 | Higher cholesterol → higher risk |
| 6 | CholCheck | +0.20 | Regular checkups associated with diagnosed individuals |
| 7 | Income | −0.14 | Higher income → lower risk |
| 8 | HvyAlcoholConsump | −0.13 | Negative association (likely confounding) |

**Conclusion:** The same variables identified in hypothesis testing are top predictors in the model, confirming both statistical and experimental consistency.

---

## 8. Assumptions
1. Missing codes (7, 9, 77, 88, 99) represent invalid or skipped responses.
2. Ordinal features like Age, Education, and GenHlth maintain their numeric rank meaning.
3. Combining prediabetes and diabetes into one category improves interpretability for binary classification.
4. Oversampling is only applied to training data to prevent test data leakage.
5. Large dataset size (~2.5 lakh) justifies very small p-values (≈0) due to high statistical power.

---

## 9. Summary
- Cleaned and preprocessed a large BRFSS dataset into structured, numeric format.
- Conducted EDA revealing realistic population trends.
- Performed statistical hypothesis testing showing significant relationships between BMI, health, and diabetes.
- Validated results through logistic regression, confirming consistent feature importance.
- Established a reproducible, data-driven framework for diabetes risk analysis.

---

**Team:** Mind and Metrics 
**Semester:** Fall 2025  
**Tools:** Python (Pandas, NumPy, Matplotlib, Seaborn, SciPy, scikit-learn)

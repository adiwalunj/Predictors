# Car Resale Predictor — Learning Project

> **Honest note:** This project has a known flaw. I'm documenting it intentionally. The rebuilt version is in progress.

---

## What I Built

A classification model to predict the number of previous owners of a used car (`Owner`: 0, 1, or 3) using features like selling price, present price, kilometers driven, fuel type, transmission, and seller type.

**Stack:** Python, scikit-learn, imbalanced-learn, pandas

**Models used:** Logistic Regression, Random Forest (with GridSearchCV tuning)

**Techniques:** SMOTE for class imbalance, StratifiedKFold cross-validation, sklearn Pipeline + ColumnTransformer

---

## Results

```
Accuracy: 95.6%

Classification Report:
              precision    recall    f1-score   support
           0       0.97      0.99      0.98        87
           1       0.00      0.00      0.00         3
```

---

## What Went Wrong

The 95.6% accuracy looks good. It isn't.

The dataset is severely imbalanced — 96.4% of samples belong to class 0 (zero previous owners). The model learned to predict class 0 every single time and got rewarded for it. Class 1 has 0% recall and 0% F1-score, meaning the model is completely blind to first-owner cars.

**Root problems identified:**

1. **Wrong problem definition.** Predicting owner count from this dataset is a poor ML problem. The target variable (`Owner`) is not what the dataset is built around — `Selling_Price` and `Present_Price` are the real signals here.

2. **SMOTE couldn't save it.** With only ~10 minority class samples, SMOTE generates synthetic points in nearly empty feature space. Not enough signal to work with.

3. **Accuracy is a misleading metric on imbalanced data.** Should have led with F1-score and ROC-AUC, not accuracy.

4. **No EDA.** Went straight to modeling without understanding the data distribution or feature relationships.

---

## What I'm Rebuilding

- Reframing as a **regression problem**: predict resale price (`Selling_Price`) given car features
- Engineering a **depreciation ratio** feature: `(Present_Price - Selling_Price) / Present_Price`
- Proper evaluation with RMSE and feature importance analysis
- EDA with visualizations before modeling

---

## Tech Used

- Python 3
- pandas, scikit-learn, imbalanced-learn
- Google Colab

---

## Status

🔴 Current version: broken (documented above)  
🔧 Rebuild: in progress

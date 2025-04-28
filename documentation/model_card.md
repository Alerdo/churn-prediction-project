# Model Card – **Random-Forest Churn Predictor**

---

## 1 · Model Description  

| Aspect | Detail |
|--------|--------|
| **Intended use** | Flag e-commerce visitors who are at high risk of never returning (churn) so that retention campaigns can be triggered. |
| **Input** | A row of 158 engineered behaviour features per *visitor* (e.g. mean session length, inter-session gap, click / purchase ratios). All inputs are numerical. |
| **Output** | *a)* **`prob_churn`** – probability in \[0, 1\] that the visitor will churn within 180 days.  <br>*b)* **`label`** – 1 = churn-risk (prob ≥ 0.50 by default), 0 = likely to return. |
| **Algorithm** | **RandomForestClassifier** (scikit-learn) – 300 trees, depth ≤ 6, `min_samples_leaf` = 20, `class_weight='balanced'`, trained on 75 % of customers and validated/tested on disjoint 25 % / 20 % splits (grouped by `visitorid`). |
| **Framework / runtime** | Python 3.11, scikit-learn 1.4.2, run inside Google Colab. |

---

## 2 · Performance  

**Evaluation metrics**

| Metric (test set) | Value |
|-------------------|-------|
| ROC AUC | **0.995** |
| Accuracy | 0.99 |
| Precision (churn = 1) | 1.00 |
| Recall (churn = 1) | 0.99 |
| Precision (non-churn = 0) | 0.68 |
| Recall (non-churn = 0) | **1.00** |

Confusion matrix (test, 757 visitors)

|          | Pred 0 | Pred 1 |
|----------|--------|--------|
| **True 0** | 17 | 0 |
| **True 1** | 9  | 731 |

*Data split:* 201 k visitors → 60 % train, 20 % validation, 20 % test, grouped so the same visitor never appears in multiple sets.

---

## 3 · Limitations  

* **Domain bound** – trained on a single European retail site (May → Sep 2015). Behaviour on other shops, seasons, or marketing regimes may differ.  
* **No demographics** – model cannot account for age, gender, or location-based effects.  
* **Cold start** – brand-new visitors with < 2 sessions may lack enough history for reliable features.  
* **Lower precision on class 0** – 32 % of non-churn warnings will be false positives at the default 0.50 threshold.

---

## 4 · Trade-offs & Ethical Considerations  

| Trade-off | Implication |
|-----------|-------------|
| **Recall ↑ vs Precision ↓** | Threshold can be raised to reduce false alerts, but recall on churners will fall. |
| **Accuracy vs Interpretability** | Random Forest outperforms a single tree by ~+1 % AUC, but is less transparent. Feature-importance or SHAP plots can mitigate. |
| **Short-term log vs Long-term drift** | Behaviour patterns may shift (COVID-era, mobile vs desktop). Periodic retraining is required to avoid performance decay. |
| **Class weight choice** | Using `class_weight='balanced'` favours catching rare churn cases; if marketing budget is limited, weights may need adjusting. |

---

*Maintainer:* Alerdo Ballabani (alerdo@example.com)  
*Last updated:* 28 Apr 2025  

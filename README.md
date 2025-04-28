# Predict-When-They-Churn  
Behaviour-Driven Customer-Loss Forecasting

---

## 🗒️ Project Overview
Online shops lose revenue when loyal buyers quietly disappear. Imagine you  want to know **who is quietly slipping away** before it’s too late.   
This project watches **how long shoppers stay**, **how often they return**, and **what they buy** to flag customers who are unlikely to purchase again during the next 180 days.  
By turning millions of raw click-stream records into clear "stay-or-leave" scores, the model can let marketing teams run retention offers **before** the customer is gone.

---

## 📦 Data
| Source | Retail Rocket E-commerce Dataset — 2.7 M events (views, carts, purchases) logged May → Sep 2015.<br>URL: <https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset> |
| Format | `events.csv`, `item_properties.csv`, `category_tree.csv` – CC BY-NC-SA 4.0 |
| Ingestion | Loaded with **PySpark** and explicit schemas to avoid silent type-casts & null creep. |
| Key note | On the items_properties when the column property is equal to 790, the corresponding data on column value its is refering to price. |

---

## 🤖 Model
| Algorithm | **RandomForestClassifier** (scikit-learn) |
| Rationale | Outperformed Logistic Regression & single Decision Trees at catching rare churners while remaining interpretable; handles nonlinear feature interactions without heavy tuning. |
| Inputs | 158 engineered features per visitor (session length stats, inter-session gaps, click / purchase ratios, recency metrics, etc.). |
| Output | `target_event` (0-1) + binary label (1 = likely churn in 30 days). |

---

## ⚙️ Hyperparameter Optimisation
Grid search (visitor-grouped 5-fold CV):  

| Parameter | Search Space | Best |
|-----------|--------------|------|
| `n_estimators` | 100 – 600 | **300** |
| `max_depth` | 4, 6, 8 | **6** |
| `min_samples_leaf` | 5, 10, 20, 40 | **20** |
| `class_weight` | `"balanced"`, custom ratios | **balanced** |

---

## 📈 Results
Test set (20 % hold-out, customer-grouped):

| Metric | Score |
|--------|-------|
| ROC AUC | **0.995** |
| Accuracy | 0.99 |
| Recall – churners | 0.99 |
| Precision – churners | 1.00 |
| Recall – non-churners | 1.00 |
| Precision – non-churners | 0.68 |




---

## 🔨 Pipeline Highlights
1. **Big-data wrangling** – PySpark for schema-safe loading, null analysis.  
2. **Date & time work** – converted epoch → UTC, removed duplicates, filled gaps.  
3. **Session creation** – events grouped by 30-minute inactivity rule.  
4. **User-level feature engineering** – 158 behaviour metrics for robust EDA.  
5. **Data cleansing** – merged tables, fixed nulls (numeric → 0, categorical → `"unknown"`).  
6. **Label generation** – churn = no purchase within next 180 days.  
7. **Model comparison** – Logistic, Decision Tree, LightGBM, CatBoost; **Random Forest** selected for best recall + stability.

---

## 🛑 Limitations
* Missing values, and null values especially on the items_properties
* Data from one shop & 2015 season – retraining needed for drift.  
* Cold-start visitors (< 2 sessions) receive lower-confidence scores.  
* Precision on non-churners (0.68) means some false alarms; threshold can be tuned per budget.

---

## 📬 Contact
[LinkedIn – Alerdo Ballabani](https://www.linkedin.com/in/alerdo-ballabani-450a85283/) • alerdo.ballabani@example.com

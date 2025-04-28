# Datasheet – **Retail Rocket E-commerce Dataset**

> Kaggle URL – <https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset>  
> Version downloaded – April 2025  

---

## 1 · Motivation  
* **Purpose** Originally released by **Retail Rocket** (a personalization-tech company) to let researchers test **session-based recommender systems** and other behaviour-prediction tasks.  
* **Creators / Funders** Data was logged by Retail Rocket’s engineering team on a live European web-shop; the company published it publicly on Kaggle to foster academic work. No external funding is listed.

---

## 2 · Composition  
* **Instances**  
  * **events.csv** – ≈ 2.76 M user–item interactions  
    * Fields: `timestamp (ms)`, `visitorid`, `event` (view / addtocart / transaction), `itemid`, `transactionid`  
  * **item_properties.csv** – ≈ 4.5 M item-property change records  
  * **category_tree.csv** – 289 category relations  
* **Entities represented** Anonymous visitors and products on a single online store.  
* **Missing data** `transactionid` is null for non-purchase events; some items lack category or property updates.  
* **Confidential / PII?** None. IDs are hashed, no names, addresses, or user attributes are present.

---

## 3 · Collection process  
* **Acquisition method** Raw click-stream was captured by the shop’s web analytics pipeline.  
* **Time window** ≈ 4 ½ months (03 May 2015 → 18 Sep 2015).  
* **Sampling** No sub-sampling reported; appears to be the full log for that period.

---

## 4 · Pre-processing & cleaning (for this project)  
* Converted Unix-epoch `timestamp` to UTC `datetime`.  
* Dropped duplicate events and obviously corrupted rows (< 0.01 %).  
* Filled missing numeric fields with 0 and left string NAs untouched.  
* Aggregated events into sessions (30-min inactivity rule) and produced 158 per-customer features (e.g., session-count ratio, inter-session gap, click / purchase counts).  
* Raw CSVs are kept alongside processed parquet files for reproducibility.

---

## 5 · Uses & limitations  
* **Typical tasks** Session-aware recommendation, conversion-rate prediction, churn prediction, sequential pattern mining.  
* **Fair-use considerations** Dataset covers a single shop & region → models may **not generalise** to other verticals or seasons.  
* **Should NOT be used** for re-identification of individuals or sensitive inferences (age, gender, etc.) since such labels are absent.

---

## 6 · Distribution  
* **Availability** Freely downloadable from Kaggle; mirrored in several academic repos.  
* **License** **CC BY-NC-SA 4.0** – non-commercial use, attribution required, share-alike. Users must also comply with Kaggle ToS.

---

## 7 · Maintenance  
* **Maintainer** Retail Rocket (dataset is static; no update schedule).  
* **Contact** analytics [at] retailrocket.net or Kaggle discussion board.  
* **Versioning** Kaggle keeps previous file versions; no separate changelog.

---

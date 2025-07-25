# **San-Diego-Traffic-Collision-Analysis**
UC Berkeley AI-ML Capstone Project

---

## **Project Overview**
This capstone project delivers a comprehensive **exploratory data analysis (EDA)** and **predictive modeling pipeline** for traffic collisions in San Diego County (2014–2023). Using data from the **California Highway Patrol’s SWITRS system**, curated by **SANDAG**, the analysis identifies key factors driving collision severity, builds baseline regression and classification models, and evaluates their effectiveness in predicting severe outcomes.

The project concludes by highlighting the challenges of modeling **rare severe crashes** and provides actionable insights for improving future models.

---

## **1 – Introduction and Business Analysis**

### **1.1 Business Understanding**
Traffic collisions impose severe human and economic costs, including injuries, fatalities, property damage, and emergency response burdens. This analysis aims to help policymakers and transportation engineers:
- Identify **high-risk conditions** and **corridors**
- Understand the role of **environmental and temporal factors**
- Support **data-driven safety interventions** aligned with Vision Zero goals

---

### **1.2 About the Data**
- **Source:** San Diego Association of Governments (SANDAG) portal  
- **Time Span:** 2014–2023  
- **Records:** ~251,000 collisions  
- **Variables:** 44 fields including date, location, casualties, collision type, weather, lighting, and contributing factors  

---

### **1.3 Research Objectives**
1. Perform **exploratory data analysis** to uncover trends and patterns in collision severity  
2. Build a **baseline regression model** to predict total casualties  
3. Extend to **classification modeling** for predicting severe collisions  
4. Interpret results to guide **safety planning and future modeling improvements**  

---

## **3 – Data Understanding**
- **Size:** 251,098 rows × 44 columns  
- **Types:** Numeric, categorical, boolean  
- **Core features:** party count, injuries, fatalities, environmental flags  
- **Missingness:** Minimal for key outcome variables; sparse geometry fields  

---

## **4 – Data Cleaning**
- Removed duplicates by `CASE ID`  
- Dropped geometry-related columns to reduce memory usage  
- Imputed missing numeric values (injuries, fatalities, party count) with zero  
- Filled categorical nulls with `"Unknown"`  
- Added binary flags for adverse conditions  

---

## **5 – Outlier Analysis**
- Used **IQR-based detection**  
- Identified extreme values (e.g., 14-party pileups, 33 injuries)  
- Capped casualties at 99th percentile for modeling while retaining original distribution for analysis  

---

## **6 – Feature Engineering**
- `Total Casualties` = Injuries + Fatalities  
- Derived binary and categorical flags:  
  - `is_weekend`, `at_intersection`, `adverse_weather`, `poor_lighting`  
  - Severity tiers: `casualty_level` (none, minor, major)  
- Encoded `day_of_week_code`  

---

## **7 – Exploratory Data Analysis**
- **Most crashes**: low-severity (0–1 casualty, ≤2 parties) under clear daylight conditions  
- **Temporal trend**: collisions peaked in 2018, dipped during the pandemic, rebounded by 2023  
- **Contextual patterns**: weekends, adverse weather, and poor lighting slightly increase casualties  
- **Correlation**:  
  - Total casualties strongly linked to injuries (r ≈ 0.99)  
  - Weak association with party count (r ≈ 0.19)  

---

## **8 – Regression Modeling**

### **Baseline Linear Regression**
- **Features:** PARTY COUNT, NUMBER KILLED, day_of_week_code, is_weekend, at_intersection, adverse_weather, poor_lighting  
- **Performance:**  
  - **MAE:** ~0.67 casualties  
  - **R²:** ~0.049 (explains only 4.9% variance)  
- **Coefficient Insights:**  
  - `NUMBER KILLED` (~0.94) dominates  
  - `PARTY COUNT` (~0.24) is secondary  
  - Contextual effects are minor  

### **Advanced Models**
- **Random Forest & XGBoost:**  
  - CV MAE: ~0.672  
  - R²: ~0.046  
- Hyperparameter tuning produced no significant gains, indicating feature limitations  

---

## **9 – Classification Modeling**
- **Target:** `is_severe_collision` = 1 if Total Casualties ≥ 2  
- **Models:** Logistic Regression, Random Forest, XGBoost  
- **Results:**  
  - Accuracy ~85% (misleading due to imbalance)  
  - Precision improved (up to ~0.63), but **Recall <1%**  
  - ROC-AUC near random (0.50–0.65)  
- **Challenge:** Severe class imbalance → almost no severe crashes detected without rebalancing methods  

---

## **10 – Conclusions and Future Recommendations**

### **10.1 Key Takeaways**
- Most collisions are low-severity; severe crashes are rare but critical  
- Contextual factors (weekend, weather, lighting) show modest impact  
- Baseline regression and advanced models achieved **low explanatory power**  
- Classification models performed poorly for severe crashes due to **extreme imbalance**  

### **10.2 Final Conclusion**
The project successfully established a **descriptive and modeling baseline** for collision severity analysis in San Diego. While predictive accuracy remains limited, these findings highlight critical directions for future work.

### **10.3 Future Recommendations**
Although this project is complete, future improvements could include:
- Richer feature sets (road type, speed limits, traffic volume)  
- One-hot encoding and interaction terms  
- Class balancing (SMOTE, weighted models)  
- Geospatial clustering for hotspot prediction  
- Advanced interpretability (SHAP, partial dependence)  
- Deployment through interactive dashboards for decision-makers  

---

## **Repository Contents**
- `San-Diego-Traffic-Collision-Analysis` – Full notebook with EDA, modeling, and results  
- `SWITRS_2023` – San Diego traffic collision dataset (2014–2023)  

---

## **View Full Notebook**
[Click here to view the Jupyter Notebook](https://github.com/walhathal/San-Diego-Traffic-Collision-Analysis/blob/main/San-Diego-Traffic-Collision-Analysis.ipynb.ipynb)

---


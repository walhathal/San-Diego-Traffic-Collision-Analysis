# San-Diego-Traffic-Collision-Analysis
UC Berkeley AI-ML Capstone Project

## Project Overview
This capstone project delivers a comprehensive exploratory data analysis and baseline predictive model for traffic collisions in San Diego County (2014–2023). Leveraging data curated by SANDAG from the California Highway Patrol’s SWITRS system, the analysis uncovers key factors influencing collision severity and establishes a linear regression benchmark for total casualty prediction.

## 1 – Introduction and Business Analysis

### 1.1 Business Understanding  
Traffic safety is a top priority for regional planners, public-safety officials, and transportation engineers. In San Diego County, collisions impose significant human and economic costs—lost productivity, emergency response expenditures, and, most importantly, injuries and fatalities. By systematically analyzing historical collision data, stakeholders can:  
- Identify high-risk corridors and intersection types  
- Understand the influence of environmental factors (weather, lighting, road geometry)  
- Guide targeted interventions (signal timing adjustments, signage improvements, enforcement campaigns)  
- Monitor the impact of safety programs over time  

Our capstone delivers data-driven insights to inform resource allocation and policy decisions, ultimately reducing collision severity and improving public safety.

### 1.2 About the Data  
This analysis leverages the San Diego Association of Governments (SANDAG) portal, which aggregates all reported collisions in San Diego County from January 1, 2014 through December 31, 2023. Every traffic crash for which a law-enforcement officer completes a collision report in the region is included. The California Highway Patrol (CHP) compiles these reports into the Statewide Integrated Traffic Records System (SWITRS). SANDAG downloads, validates, and enriches the SWITRS subset for San Diego, ensuring geographic accuracy and consistency for regional planning. Key attributes include:  
- **Temporal span:** 2014–2023  
- **Geography:** All incorporated and unincorporated areas of San Diego County  
- **Core variables:** collision date/time, primary/secondary road identifiers, latitude/longitude, injury and fatality counts, environmental conditions (weather, lighting), collision severity, party counts  

### 1.3 Research Objectives  
1. **Exploratory Data Analysis**  
   - Uncover patterns in injury and fatality distributions  
   - Examine correlations with environmental and temporal factors  
2. **Data Preparation**  
   - Clean missing or duplicate entries  
   - Engineer features (e.g. total casualties, day-of-week flags)  
3. **Baseline Modeling**  
   - Develop a regression model to predict total casualties  
   - Evaluate using Mean Absolute Error (MAE) and R²  
4. **Actionable Insights**  
   - Highlight key risk factors for targeted interventions  
   - Provide recommendations for future data collection and analysis  

## 3 – Data Understanding  
- **Shape:** 251 098 rows × 44 columns  
- **Variable groups:** temporal & IDs, location & geometry, collision details, casualty counts, environmental flags  
- **Data types:** mixture of numeric, categorical (object), and boolean fields  
- **Missing patterns:** sparse geometry fields (latitude/longitude), some violation categories; core casualty fields are fully populated  

## 4 – Data Cleaning  
- **Duplicates:** dropped by `CASE ID`  
- **Geometry:** removed shapefile/projection columns to reduce memory (Shape, X, Y, Location sandag, Reservation sandag)  
- **Numeric imputation:** filled `NUMBER INJURED`, `NUMBER KILLED`, `PARTY COUNT` with zero; median-imputed `DISTANCE`; dropped overly sparse fields  
- **Categorical imputation:** replaced missing values in key strings (e.g. WEATHER, LIGHTING, COLLISION SEVERITY) with “Unknown”  
- **Boolean flags:** assumed false when missing for fields like `PEDESTRIAN ACCIDENT`, `ALCOHOL INVOLVED`  

## 5 – Outlier Analysis  
- **Method:** IQR-based detection (multiplier 1.5) on PARTY COUNT, NUMBER INJURED, NUMBER KILLED, DISTANCE, total casualties  
- **Findings:** small proportion of extreme values (e.g. multi-vehicle pileups, mass-casualty events, implausible distances)  
- **Handling strategy:**  
  - **Flag** genuine high-severity events for subgroup analysis  
  - **Cap** casualties at the 99th percentile for modeling  
  - **Impute** implausible distance entries with median  

## 6 – Feature Engineering  
Created actionable, interpretable variables:  
- `total_casualties` = injuries + fatalities  
- Binary flags: `is_fatal`, `is_multi_casualty`, `is_severe_collision`, `adverse_weather`, `poor_lighting`, `is_weekend`, `at_intersection`  
- Ordinal categories: `casualty_level` (none, minor, major), `party_cat` (small, medium, large), `distance_cat` (near, mid, far)  
- Temporal codes: `accident_year`, `day_of_week_code`  

## 7 – Exploratory Data Analysis  
- **Univariate:** histograms and boxplots show heavy right-skew in casualties and party counts  
- **Categorical counts:** majority of collisions are low-severity (0–1 casualties, ≤2 parties), in clear/daylight conditions  
- **Bivariate:** weak positive association between party count and casualties; slightly higher casualties under adverse weather and on weekends  
- **Temporal trends:** collision volume peaked pre-pandemic, dipped in 2020, then rebounded; casualties per crash decreased until 2018 and rose thereafter  
- **Correlation:** `total_casualties` driven almost entirely by injuries (r ≈ 0.99), weak relation to party count (r ≈ 0.19) and fatalities (r ≈ 0.11)  

## 8 – Baseline Modeling  
- **Model:** Linear Regression predicting `total_casualties` using seven features (`PARTY COUNT`, `NUMBER KILLED`, `day_of_week_code`, `is_weekend`, `at_intersection`, `adverse_weather`, `poor_lighting`)  
- **Performance:**  
  - MAE ≈ 0.67 casualties  
  - R² ≈ 0.049  
- **Coefficients:**  
  - `NUMBER KILLED` (~0.94) strongest predictor  
  - `PARTY COUNT` (~0.24) secondary  
  - Contextual flags have small linear effects  

## 9 – Conclusions  
- Most collisions are low-severity; rare high-severity events drive policy urgency  
- Weekend and adverse-condition crashes exhibit modestly higher severity  
- Linear baseline provides a clear benchmark: MAE < 0.67, R² > 0.049 for future models  
- **Next steps:** feature expansion, nonlinear algorithms, cross-validation & tuning, interpretability (SHAP), and stakeholder dashboard  

---

## Link to Jupyter Notebook

[Click here to view the notebook](https://github.com/walhathal/Comparing-Classifiers/blob/main/prompt_III.ipynb)

## Author

Wael Alhathal - University of California, Berkeley - AI/ML Student

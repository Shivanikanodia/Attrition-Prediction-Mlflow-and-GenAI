### End-to-End Employee Attrition Prediction Pipeline on Databricks (MLflow, Unity Catalog and GenAI)

Attrition and Turnover Rate refers to the rate at which employees leave a company. Employee turnover is a costly problem for organizations. The cost of replacing an employee can be quite large, and a study found that companies typically pay about one-fifth of an employee's salary to replace them. 

The cost can significantly increase if executives or highest-paid employees are to be replaced. The cost of replacing employees for most employers remains significant. This is due to the amount of time spent to interview and find a replacement, sign-on bonuses, and the loss of productivity for several months while the new employee gets accustomed to the new role. 

--- 

#### Problem Statement:

HR Leaders and HRBPs face high attrition and rising hiring costs without clear visibility into high-risk segments or key attrition drivers. This project builds a data-driven framework to predict employee attrition, assess financial impact, and recommend targeted retention strategies.

This is a supervised binary classification problem where:

0 → Likely to stay

1 → Likely to leave

The target variable represents the probability of an employee leaving the company.

---

### Project Structure

**Data Engineering:**  Data exploration and storage in Unity Catalog using Medallion Architecture (Bronze, Silver, Gold).

**EDA:** Distribution analysis, summary statistics, skewness, and visualizations.

**Feature Selection:** Statistical testing (A/B testing) to identify key predictors and store feature tables.

**Preprocessing:** Encoding, scaling, and train-test split.

**Modeling:** Train and evaluate classification models using confusion matrix and classification report.

**MLflow Tracking:** Log parameters, metrics, artifacts, and model files for reproducibility.

**GenAI Integration:** Use Databricks SDK (WorkspaceClient) to generate natural language explanations of attrition risk, financial impact, and recommended intervention strategies.

This enables proactive retention planning and optimized HR budget allocation.

---

#### Dataset Overview: 

The dataset includes employee demographics, Job role information, Engagement, and performance-related metrics.
Performed categorical feature analysis by examining unique values and their frequency distributions to understand representation and potential imbalance across categories.
Identified and handled missing values and empty strings using isnull().sum() and appropriate data-cleaning strategies.
Numerical features such as Distance from Home, Monthly Income, Years at Company, Years Since Last Promotion, and Total Working Years exhibited right-skewed distributions. 

---

#### Unity Catalog & Medallion Architecture:

The project follows a Bronze–Silver–Gold medallion architecture to ensure scalable, reliable, and reusable ML features.

• Bronze: Raw employee event data ingested as Delta tables. **(bronze-emplpoyee_raw)** 
• Silver: Cleaned, standardized, and validated datasets. **(silver_employee_clean)**
• Gold: Feature-engineered and analytics-ready tables used for ML training and inference. **(employee_interactions)**

Created a Unity Catalog catalog (employee_attrition_catalog) and schema (attrition_schema) to store Bronze, Silver, and Gold Delta tables, enabling governed, scalable, and versioned access to datasets used across the ML workflow.

<img width="662" height="298" alt="image" src="https://github.com/user-attachments/assets/6204d8cc-c4e2-49d3-9fb8-f6434b8d1601" />

---

### Feature Selection:

- Feature selection was performed to identify the most predictive and stable drivers of employee attrition while reducing noise and multicollinearity.
- Applied Chi-square tests to evaluate the association between categorical features (e.g., job role, overtime, work-life balance) and employee attrition.
- Used two-sample t-tests to assess statistically significant differences in continuous features (e.g., monthly income, years at company, distance from home) between attrition and non-attrition groups.
- Combined statistical test results with domain knowledge to ensure retained features were both predictive and interpretable for business stakeholders.
- Validated feature importance consistency across Logistic Regression, Random Forest, and XGBoost models to reduce model-specific bias.
___

#### MODEL SELECTION:

- Logistic regression with L1 and L2 Penality, and class weight balanced to account for coefficents and features.
- Random forest classiffier with hyperparamters like max_depth, n_estimators which will help us to generalize well, and deal with variance and overfitting. 
- Xgboost Classifier to deal with complex data points. 

---

#### Model Experimentation using MLFLOW:

Setting the Experimentation inside Databricks Notebook. 

<img width="1039" alt="Experimentation" src="https://github.com/user-attachments/assets/a927d8d0-5ea7-4e53-8365-fdb842b5bd62" />

This centralized tracking ensured experiment reproducibility and logging Hyperparameter from models, metrics, artifacts and model versioning. 
Logged key hyperparameters, evaluation metrics, trained model and visual artifacts like confusion matrix for every run — making it easy to reproduce or explain later.

---

#### MLFLOW METRICS:  

As we can see screenshot below from Databricks MLFlow UI with Run Name, Duration of each Run and metrics logged.  Used the MLflow UI in Databricks to compare multiple runs of Logistic Regression, Random Forest, and XGBoost. 

<img width="1264" height="440" alt="Screenshot 2025-10-07 at 11 25 24" src="https://github.com/user-attachments/assets/1ef7910e-319a-4d11-9ffd-5691308bcfb8" />

---

### MODEL REGISTERED: 

The selected model was registered in the Databricks Model Registry and deployed via Databricks Model Serving to support real-time inference. 

<img width="1652" height="770" alt="image" src="https://github.com/user-attachments/assets/337de7ee-528b-47fe-b096-f43e4f133c5f" />


### MODEL SERVED: 

The endpoint retrieves the latest Gold-layer features, generates attrition risk scores, and returns predictions, enabling integration with downstream applications and monitoring pipelines.


<img width="2352" height="606" alt="image" src="https://github.com/user-attachments/assets/6661d742-a47f-433c-9773-8bd5929310fc" />


---

### GenAI app for making real time predictions and recommendations:

<img width="2054" height="680" alt="image" src="https://github.com/user-attachments/assets/8b65cbcb-f6d4-4c2e-9754-95e769850f57" />

---

### Finacial Impact Modeling using Genie to infer from UC Tables:


Attrition Risk Score × Replacement Cost × Headcount = Estimated Financial Risk Exposure



<img width="729" height="135" alt="Screenshot 2026-02-11 at 22 33 35" src="https://github.com/user-attachments/assets/689ee710-46c0-481d-b591-85d6462a82ca" />





<img width="1134" height="409" alt="Screenshot 2026-02-11 at 22 34 43" src="https://github.com/user-attachments/assets/6f529950-1e8c-4a04-8221-ff77067eca58" />




## 📊 Model Performance Snapshot

| Model | Precision | Recall | F1 Score |
|-----|---------|-------|--------|
| Logistic Regression | 0.62 | 0.74 | 0.67 |
| Random Forest | 0.70 | 0.82 | 0.75 |
| XGBoost | 0.68 | 0.80 | 0.73 |

---

### HIGH RISK FACTORS FROM MODEL AND RECOMMENDATIONS TO HR TEAMS: 

- Overtime, Low Job satisfaction and Poor work-life balance - Monitor satisfaction & workload monthly.
- Employees with long commutes or frequent travel - Enable flexible work or hybrid policies.
- Employees with limited growth, low income or recognition  - Design retention bonuses or stock options.
   
--- 

## Why This Project Matters:

This project operationalizes machine learning in People Analytics by combining scalable data architecture, reproducible experimentation, explainability, and real-time serving—bridging the gap between data science and production ML.

It enables HR leaders to prioritize interventions based on financial risk, translates predictions into executive-ready cost metrics, improves transparency through SHAP-based explainability, reduces reactive hiring via probabilistic attrition forecasting, and integrates predictive ML with Generative AI to automate risk insights and recommendations
  
---

### 📁 Clone the repository
git clone https://github.com/<ShivaniKanodia>/employee-attrition-mlpipeline.git
cd employee-attrition-mlpipeline

### 📦 Install Python dependencies
pip install -r requirements.txt

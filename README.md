### End-to-End Employee Attrition Prediction Pipeline on Databricks (MLflow, Unity Catalog, SHAP, GenAI)

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

**Explainability:** SHAP analysis to identify key attrition drivers.

**GenAI Integration:** Use Databricks SDK (WorkspaceClient) to generate natural language explanations of attrition risk, financial impact, and recommended intervention strategies.

This enables proactive retention planning and optimized HR budget allocation.

---

#### Dataset Overview: 

The dataset includes employee demographics, Job role information, Engagement, and performance-related metrics.
Performed categorical feature analysis by examining unique values and their frequency distributions to understand representation and potential imbalance across categories.
Identified and handled missing values and empty strings using isnull().sum() and appropriate data-cleaning strategies.
Numerical features such as Distance from Home, Monthly Income, Years at Company, Years Since Last Promotion, and Total Working Years exhibited right-skewed distributions. 
Applied log1p transformation to reduce skewness and stabilize variance for improved model performance.

---

#### Unity Catalog & Medallion Architecture:

The project follows a Bronze–Silver–Gold medallion architecture to ensure scalable, reliable, and reusable ML features.

• Bronze: Raw employee event data ingested as Delta tables  
• Silver: Cleaned, standardized, and validated datasets  
• Gold: Feature-engineered and analytics-ready tables used for ML training and inference

Created a Unity Catalog catalog (Attrition_catalog) and schema (Attrition_schema) to store Bronze, Silver, and Gold Delta tables, enabling governed, scalable, and versioned access to datasets used across the ML workflow.

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

#### MLFLOW METRICS AND DASHBOARD:  

As we can see screenshot below from Databricks MLFlow UI with Run Name, Duration of each Run and metrics logged.  Used the MLflow UI in Databricks to compare multiple runs of Logistic Regression, Random Forest, and XGBoost. 



<img width="1264" height="440" alt="Screenshot 2025-10-07 at 11 25 24" src="https://github.com/user-attachments/assets/1ef7910e-319a-4d11-9ffd-5691308bcfb8" />




Each dashboard recorded: adjusted_f1, adjusted_precision, adjusted_recall, precision, recall and f1 score. 





<img width="908" height="363" alt="Screenshot 2025-10-07 at 07 30 50" src="https://github.com/user-attachments/assets/fcd14556-35a6-40a7-a289-db8e17fb9729" />






<img width="928" height="362" alt="Screenshot 2025-10-07 at 07 31 20" src="https://github.com/user-attachments/assets/f5446022-0a39-4c09-98ee-10de40d85eda" />




---



<img width="2368" height="876" alt="image" src="https://github.com/user-attachments/assets/a538168b-26c0-40fe-825c-e02ad9731f9a" />




### MODEL SERVING AND REGISTRY: 

The selected model was registered in the Databricks Model Registry and deployed via Databricks Model Serving to support real-time inference. The endpoint retrieves the latest Gold-layer features, generates attrition risk scores, and returns predictions in JSON format, enabling integration with downstream applications and monitoring pipelines.



<img width="632" height="227" alt="Screenshot 2025-10-18 at 20 46 12" src="https://github.com/user-attachments/assets/30af9741-4000-4161-b440-9446459431aa" />




<img width="2352" height="606" alt="image" src="https://github.com/user-attachments/assets/6661d742-a47f-433c-9773-8bd5929310fc" />

---

### GenAI app using Served Model for making real time predictions and recommendations:





<img width="2054" height="680" alt="image" src="https://github.com/user-attachments/assets/8b65cbcb-f6d4-4c2e-9754-95e769850f57" />




---

## 📊 Model Performance Snapshot

| Model | Precision | Recall | F1 Score |
|-----|---------|-------|--------|
| Logistic Regression | 0.62 | 0.74 | 0.67 |
| Random Forest | 0.70 | 0.82 | 0.75 |
| XGBoost | 0.68 | 0.80 | 0.73 |

### Conclusion:

- A tuned Random Forest model (max_depth = 6, n_estimators = 200) delivered the best performance, achieving 82% recall and 70% precision with a threshold of 0.33.
- Unity Catalog enabled governed data access, SHAP provided explainability, and MLflow ensured reproducible training, tracking, and deployment.
---

### HIGH RISK FACTORS FROM MODEL AND RECOMMENDATIONS TO HR TEAMS: 

- Overtime, Low Job satisfaction and Poor work-life balance - Monitor satisfaction & workload monthly.
- Employees with long commutes or frequent travel - Enable flexible work or hybrid policies.
- Employees with limited growth, low income or recognition  - Design retention bonuses or stock options.
   
--- 


## 💡 Why This Project Matters:

This project operationalizes machine learning in People Analytics by combining scalable data architecture, reproducible experimentation, explainability, and real-time serving—bridging the gap between data science and production ML.

It enables HR leaders to prioritize interventions based on financial risk, translates predictions into executive-ready cost metrics, improves transparency through SHAP-based explainability, reduces reactive hiring via probabilistic attrition forecasting, and integrates predictive ML with Generative AI to automate risk insights and recommendations

---

### Tech Stack:

- **Languages**: Python  
- **Libraries**: `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`, `mlflow`, `shap`  
- **Platform**: Databricks  
- **Version Control & Tracking**: MLflow. 
  
---

### 📁 Clone the repository
git clone https://github.com/<ShivaniKanodia>/employee-attrition-mlpipeline.git
cd employee-attrition-mlpipeline

### 📦 Install Python dependencies
pip install -r requirements.txt

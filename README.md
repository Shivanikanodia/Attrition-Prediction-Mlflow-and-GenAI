### Workforce Attrition Prediction with Explainable & Production-Ready ML

• Built a production-style ML pipeline on Databricks to predict employee attrition using Logistic Regression, Random Forest, and XGBoost  
• Implemented Bronze–Silver–Gold medallion architecture with Unity Catalog for governed, versioned ML features  
• Tracked and registered models using MLflow, achieving 82% recall and 70% precision with explainable SHAP insights  
• Deployed the model via Databricks Model Serving for real-time inference

---

#### Problem Statement:

This project builds an end-to-end, production-ready ML system to predict employee attrition and surface key drivers using explainable and reproducible machine learning practices.

---

###  End-to-End ML Pipeline:

Data Management with Unity Catalog

Data Cleaning & Validation

Feature Engineering & Selection

Model Training & Evaluation

Experiment Tracking & Monitoring (MLflow)

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

Created a Unity Catalog catalog (ml_catalog) and schema (ml_schema) to store Bronze, Silver, and Gold Delta tables, enabling governed, scalable, and versioned access to datasets used across the ML workflow.

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

### MODEL SERVING AND REGISTRY: 

The selected model was registered in the Databricks Model Registry and deployed via Databricks Model Serving to support real-time inference. The endpoint retrieves the latest Gold-layer features, generates attrition risk scores, and returns predictions in JSON format, enabling integration with downstream applications and monitoring pipelines.


<img width="632" height="227" alt="Screenshot 2025-10-18 at 20 46 12" src="https://github.com/user-attachments/assets/30af9741-4000-4161-b440-9446459431aa" />

---

### GenAI app using Served Model for making real time predictions and recommendations:



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


## 💡 Why This Project Matters
This project demonstrates how machine learning can be operationalized responsibly in people analytics by combining scalable data architecture, reproducible experimentation, explainability, and real-time serving—bridging the gap between data science and production ML systems.


### Future Scope:

• Automate retraining pipelines using Databricks Jobs with scheduled evaluation and rollback strategies  
• Enable continuous monitoring of feature drift and prediction quality  
• Extend the system with an AI agent for proactive alerts and retention recommendations

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

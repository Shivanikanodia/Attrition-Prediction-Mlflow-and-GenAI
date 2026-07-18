# 🚀 End-to-End Employee Attrition Prediction Pipeline on Databricks  
### MLflow | Unity Catalog | Model Serving | Generative AI Integration

---

## 📌 Business Problem

Employee attrition significantly increases hiring, onboarding, and productivity costs. Research indicates replacing an employee can cost up to **20-150%% of their annual salary**, with even higher impact for senior roles.

HR teams often lack visibility into:
- Proactive monitoring of High-risk employee segments
- Key drivers of attrition for department/team/employee level
- Financial risk by department - (In highly specialised roles loosing even a single employee can cost significant amount as they take away knowledge, productivity and recruiters need 3-4 months to fill the role)

This project builds a **production-ready, end-to-end ML pipeline** to predict attrition risk by Department/Job Role, key drivers across employee cohorts, quantify financial impact, and recommend targeted retention strategies (low cost and high impact) using 30-60-90 days playbook.
It demonstrates how we measure impact of retention strategies using A/B Testing by creating matched cohort groups to isolate the actual impact of change.

---

## 🎯 Objective

This is a supervised binary classification problem:

- `0` → Likely to Stay  
- `1` → Likely to Leave  

The model outputs the probability of attrition for each employee.

---

# 🏗 Architecture Overview

## 🔹 Medallion Architecture (Unity Catalog Governed)

| Layer  | Description                              | Table Name                |
|--------|------------------------------------------|---------------------------|
| Bronze | Raw ingested employee data               | `bronze_employee_raw`     |
| Silver | Cleaned and standardized dataset         | `silver_employee_clean`   |
| Gold   | Feature-engineered ML-ready dataset      | `employee_interactions`   |

Created:
- **Catalog:** `employee_attrition_catalog`
- **Schema:** `attrition_schema`

This ensures governed, scalable, and version-controlled data access.

<img width="662" height="298" alt="image" src="https://github.com/user-attachments/assets/086b759d-46aa-4e52-98b6-ac4265249036" />


---

# 📊 Dataset Overview

The dataset includes:
- Employee demographics like age, gender and ethinicity (These are not passed to model to ensure we prevent biasness)
- Job role, department and reporting manager information
- Promotion and Compensation variables
- Engagement scores from pulse surveys 
- Performance rating (self vs manager)

### Data Preparation

Bronze Layer: 
- Checked data freshness, schema, data types and critical fields.
- Enfored data quality checks like matching records to source systems, dropping null id, negatives values in tenure and salaries and invalid categories and reasons for attrition reason.
- implemented de-duplication logic to keep only the latest record per employee ID based on the system ingestion timestamp (_ingested_at).

  Silver Layer:
- Handled missing values in promotion and internal moblity counts and fixed outliers for distance from home and years since last promotion using capping at 99th percentile.
- Transformed currencies from across the globe to baseline using Exchange rate and timespamp from 10+ multi-regions to UTC to ensure consistency and accuracy in dowmstream modeling.
- Standardised 400+ vague job tittles into 12 Job familties. 
- Ensured valid exit reasons are mapped to right attrition category ("voluntry, regrettable, involuntry"). 
- Cleaned and validated data in Silver layer along with data masking and row level security to ensure manager from one region only sees his region employee records.

 Gold Layer:
- Addressed right-skewed numerical features using log transformation to compress large values and expand small values
- Transformed categorical feature for numerical using pd.dummies to ensure error-free modeling for logistic classifier.
- Applied Standard Scaling to bring values to same scale (Age and Salary) 

---

# 🔍 Feature Selection

Feature selection was performed using statistical testing combined with HR domain knowledge.

### Methods Used:
- **Chi-square tests** for categorical variables like overtime and gender
- **Two-sample t-tests** for continuous variables like job satisfaction scores, monthly income and years since last promotion

### Key Predictors Identified across organisation:

- Overtime  
- Job Satisfaction & Work-Life Balance  
- Years at Company & Years Since Last Promotion 
- Monthly Income  

---

# ⚙️ Modeling & Experimentation (MLflow)

<img width="1264" height="440" alt="498590430-1ef7910e-319a-4d11-9ffd-5691308bcfb8" src="https://github.com/user-attachments/assets/5b2202ff-8686-434b-9eb7-f87b53b18468" />


## Models Compared:
- Logistic Regression (L1/L2 Regularization + Class Weight Balanced)
- Decision Tree Classifier
- Random Forest Classifier
- XGBoost Classifier

## Handling Class Imbalance:
- Applied **SMOTE** to improve minority class representation

## Threshold Optimization:
Instead of relying on the default 0.5 cutoff, decision thresholds were tuned to prioritize **recall**.

> Missing a high-risk employee is more costly than investigating a false positive.

All parameters, metrics, artifacts, and models were logged using **MLflow**.

---

## 📈 Model Performance

| Model | Precision | Recall | F1 Score |
|--------|------------|--------|----------|
| Logistic Regression | 0.62 | 0.74 | 0.67 |
| Random Forest | 0.70 | 0.82 | **0.75** |
| XGBoost | 0.68 | 0.80 | 0.73 |

🏆 **Selected Model: Random Forest**

---

# 🧪 Experiment Tracking

Using MLflow:
- Logged hyperparameters
- Tracked evaluation metrics
- Stored confusion matrices
- Versioned trained models
- Registered best model in Model Registry

Ensures reproducibility and experiment governance.



---

# 🚀 Model Deployment

- Registered in **Databricks Model Registry**


<img width="1652" height="770" alt="image" src="https://github.com/user-attachments/assets/8a64a7d6-84ff-439a-9a9a-c56d4c2fc01a" />


- Deployed via **Databricks Model Serving**


<img width="2048" height="527" alt="image" src="https://github.com/user-attachments/assets/1f080ff4-6aac-4ab6-ad84-b8cd98d96b69" />


- Supports real-time attrition risk inference

The endpoint retrieves Gold-layer features and returns attrition probability prediction using version 3 model. 

---

# 💰 Financial Impact Modeling:

Attrition probabilities were translated into expected financial exposure, for each department. 

<img width="848" height="134" alt="Screenshot 2026-02-13 at 17 10 42" src="https://github.com/user-attachments/assets/e8e2f707-fb91-4871-beb0-5bdf23ae6db9" />


### Formula:

```

We used an industry benchmark of approximately 1.5x of annual salary as replacement cost, which accounts for hiring, onboarding, productivity loss, and specialised knowledge loss.


Individual Expected Costᵢ = P(Attritionᵢ) × Replacement Costᵢ  
Total Replacement cost (By Department) = SUM (P × Replacement Cost)  
Expected Attrition Volume = Σ P(Attritionᵢ) - ( Sum of Bernoulli expectations = sum of probabilities)
```

### Outputs:
- Expected attrition volume
- Total expected replacement cost
- Percentage of high-risk employees per department

1. Model → Probability 

2. Probability → Expected Exits

3. Expected Exits → Expected Cost

4. Expected Cost → Department Financial Exposure


---

# 🤖 Generative AI Integration

Integrated Databricks LLM capabilities to convert model outputs into executive-ready insights using databricks-llama-4-maverick.

Generated automatically:
- Risk explanations and key drivers
- Targeted retention strategies
- timeline
- Success metrics 

Example:
If competitive Job Market and High Demand of Skills are key drivers of attrition for R&D Department, the system recommends Career Developement, compensation and benefits reviews for industry standards and Growth opportunities.

This would enable HRBP's to allocate budget on targeted retention strategies proactively and priortise business critical roles.  

<img width="2122" height="744" alt="image" src="https://github.com/user-attachments/assets/7100abde-b978-4d1d-bd07-12444e0b37d6" />


---

# 📊 Key Insight

The **Research & Development department** showed the highest projected attrition exposure, making it a priority for intervention planning.

Databricks Genie enables natural language queries over governed Unity Catalog tables, allowing leaders to ask:

> “What is percentage of high risk employees, top key drivers and cost associated?”

<img width="1249" height="503" alt="Screenshot 2026-02-13 at 17 20 24" src="https://github.com/user-attachments/assets/d729cca7-bbdc-49dc-8356-a2afd7356894" />


---

# 🌟 Why This Project Matters

This solution operationalizes machine learning in People Analytics by combining:

- Scalable data architecture
- Governed feature engineering
- Reproducible ML experimentation
- Cost-sensitive threshold tuning
- Real-time model serving
- Financial risk quantification
- Generative AI-powered insights

It bridges the gap between data science experimentation and production ML systems.

---

# 📁 Clone the Repository

git clone https://github.com/Shivanikanodia/Attrition-Prediction-Mlflow-and-GenAI.git

cd Attrition-Prediction-Mlflow-and-GenAI

pip install -r requirements.txt

---

# 📦 Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 📌 Tech Stack

- Python
- Scikit-learn
- XGBoost
- Databricks
- MLflow
- Unity Catalog
- Delta Tables
- Databricks Model Serving
- Generative AI (LLM Integration)

---


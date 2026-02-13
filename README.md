# 🚀 End-to-End Employee Attrition Prediction Pipeline on Databricks  
### MLflow | Unity Catalog | Model Serving | Generative AI Integration

---

## 📌 Business Problem

Employee attrition significantly increases hiring, onboarding, and productivity costs. Research indicates replacing an employee can cost up to **20% of their annual salary**, with even higher impact for senior roles.

HR teams often lack visibility into:
- High-risk employee segments
- Key drivers of attrition
- Financial exposure by department

This project builds a **production-ready, end-to-end ML pipeline** to predict attrition risk, quantify financial impact, and recommend targeted retention strategies.

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

---

# 📊 Dataset Overview

The dataset includes:
- Employee demographics
- Job role information
- Engagement metrics
- Performance-related attributes

### Data Preparation
- Handled missing values and empty strings
- Analyzed categorical distributions for imbalance
- Addressed right-skewed numerical features
- Cleaned and validated data in Silver layer

---

# 🔍 Feature Selection

Feature selection was performed using statistical testing combined with HR domain knowledge.

### Methods Used:
- **Chi-square tests** for categorical variables  
- **Two-sample t-tests** for continuous variables  

### Key Predictors Identified:
- Overtime  
- Job Satisfaction  
- Work-Life Balance  
- Years at Company  
- Monthly Income  

---

# ⚙️ Modeling & Experimentation (MLflow)

## Models Compared:
- Logistic Regression (L1/L2 Regularization + Class Weight Balanced)
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
- Deployed via **Databricks Model Serving**
- Supports real-time attrition risk inference

The endpoint retrieves Gold-layer features and returns attrition probability predictions.

---

# 💰 Financial Impact Modeling

Attrition probabilities were translated into expected financial exposure.

### Formula:

```
Individual Expected Costᵢ = P(Attritionᵢ) × Replacement Costᵢ  
Total Department Exposure = Σ (P × Replacement Cost)  
Expected Attrition Volume = Σ P(Attritionᵢ)
```

### Outputs:
- Expected attrition volume
- Total expected replacement cost
- Percentage of high-risk employees per department

---

# 🤖 Generative AI Integration

Integrated Databricks LLM capabilities to convert model outputs into executive-ready insights.

Generated automatically:
- Risk explanations
- Financial impact summaries
- Targeted retention strategies

Example:
If overtime and low job satisfaction are key drivers, the system recommends workload monitoring and flexible work policies.

---

# 📊 Key Insight

The **Research & Development department** showed the highest projected attrition exposure, making it a priority for intervention planning.

Databricks Genie enables natural language queries over governed Unity Catalog tables, allowing leaders to ask:

> “Which department should prioritize intervention this quarter?”

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

```bash
git clone https://github.com/<ShivaniKanodia>/employee-attrition-mlpipeline.git
cd employee-attrition-mlpipeline
```

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

Expected Attrition Volume=∑P(Attritioni)\textbf{Expected Attrition Volume} = \sum P(\text{Attrition}_i)Expected Attrition Volume=∑P(Attritioni​)


<img width="729" height="135" alt="Screenshot 2026-02-11 at 22 33 35" src="https://github.com/user-attachments/assets/689ee710-46c0-481d-b591-85d6462a82ca" />


<img width="1134" height="409" alt="Screenshot 2026-02-11 at 22 34 43" src="https://github.com/user-attachments/assets/6f529950-1e8c-4a04-8221-ff77067eca58" />

<img width="2054" height="680" alt="image" src="https://github.com/user-attachments/assets/8b65cbcb-f6d4-4c2e-9754-95e769850f57" 

<img width="2352" height="606" alt="image" src="https://github.com/user-attachments/assets/6661d742-a47f-433c-9773-8bd5929310fc" />

<img width="1652" height="770" alt="image" src="https://github.com/user-attachments/assets/337de7ee-528b-47fe-b096-f43e4f133c5f" />


<img width="1264" height="440" alt="Screenshot 2025-10-07 at 11 25 24" src="https://github.com/user-attachments/assets/1ef7910e-319a-4d11-9ffd-5691308bcfb8" />


<img width="1039" alt="Experimentation" src="https://github.com/user-attachments/assets/a927d8d0-5ea7-4e53-8365-fdb842b5bd62" 

img width="662" height="298" alt="image" src="https://github.com/user-attachments/assets/6204d8cc-c4e2-49d3-9fb8-f6434b8d1601" />



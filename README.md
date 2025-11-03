# 🩺 Health Risk Analysis using NHANES Data (2011–2023)

## 📘 Project Overview
This project analyzes **NHANES (National Health and Nutrition Examination Survey)** data from 2011 to 2023 to explore how lifestyle, demographic, and clinical factors affect the risk of chronic diseases — especially **diabetes** and **hypertension**.  

The aim is to:
- Identify relationships between health indicators (e.g., BMI, HbA1c, Blood Pressure) and behaviors (e.g., smoking, alcohol use).
- Build a **predictive model** that estimates an individual's **health risk level** based on these factors.
- Present insights through an **interactive Streamlit dashboard**.

---

## 🎯 Research Objectives
1. Analyze demographic, behavioral, and clinical factors influencing chronic disease risks.  
2. Investigate correlations between BMI, blood pressure, and glycohemoglobin (HbA1c).  
3. Explore socioeconomic effects (education, income) on health outcomes.  
4. Develop a machine learning model to predict individual health risk levels.  

---

## 🧩 Dataset Description
Data Source: [NHANES (CDC)]([https://www.cdc.gov/nchs/nhanes/index.htm](https://wwwn.cdc.gov/nchs/nhanes/default.aspx))  
Years Covered: **2011 – 2023**  
Merged Datasets:
- Demographics  
- Alcohol Use  
- Smoking  
- Body Measures  
- Blood Pressure  
- Glycohemoglobin (HbA1c)  
- Diabetes  
- Income  
- Education  
- Reproductive Health  

Final merged dataset: **57,395 participants × 20 columns**

---

## 🧹 Project Workflow
### 1️⃣ Data Extraction
- Automated reading of `.xpt` SAS files using `pandas.read_sas()`.
- Merging across datasets using respondent ID (`SEQN`).
- Combining multiple NHANES cycles (2011–2023).

### 2️⃣ Data Cleaning
- Handling missing values and data inconsistencies.  
- Mapping categorical codes to readable labels.  
- Calculating BMI categories and blood pressure classification (AHA & ESH).  

### 3️⃣ Exploratory Data Analysis (EDA)
- Descriptive statistics of key indicators (BMI, BP, HbA1c).  
- Correlation heatmap between demographic and health factors.  
- Visual insights (gender, income, smoking vs chronic conditions).  

### 4️⃣ Modeling
- Logistic Regression / Random Forest for health risk prediction.  
- Evaluation using accuracy, precision, recall, and AUC score.  

### 5️⃣ Visualization & Dashboard
- Streamlit app with:
  - Filters (gender, smoking, education, etc.)
  - Summary metrics  
  - Distribution and correlation plots  
  - Risk prediction interface  

---

## 📊 Tech Stack
| Category | Tools |
|-----------|--------|
| Data Extraction | Python, Pandas |
| Data Cleaning | Pandas, NumPy |
| Visualization | Seaborn, Matplotlib, Plotly |
| Modeling | Scikit-learn |
| Dashboard | Streamlit |
| Documentation | Markdown, GitHub |

---

## 🧠 Key Insights (Expected)
- High BMI, elevated HbA1c, and smoking strongly correlate with high-risk categories.  
- Education and income show protective effects against chronic diseases.  
- Age and gender differences are significant predictors in the model.  

---

## 🚀 How to Run
1. Clone this repository:
   ```bash
   git clone https://github.com/<username>/NHANES-Health-Risk-Analysis.git

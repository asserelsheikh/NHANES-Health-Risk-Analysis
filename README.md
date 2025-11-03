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
Data Source: [NHANES (CDC)](https://wwwn.cdc.gov/nchs/nhanes/default.aspx)  
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


## 1. Methodology

The project follows a structured data analysis pipeline, starting with the raw `NAHNES.csv` and resulting in the final cleaned `NHANES_cleaned.csv` dataset.

### 1.1. Data Cleaning and Preprocessing

The raw data was extensively cleaned to prepare it for analysis:

* **Variable Mapping:** Encoded categorical variables (e.g., `1`, `2`, `7`, `9`) were mapped to human-readable string values (e.g., `'Male'`, `'Female'`, `'Yes'`, `'No'`, `'Refused'`).
* **Logical Imputation (Smart Fill):** Missing values were imputed based on domain knowledge and logical relationships between variables:
    * **Smoking:** Participants who answered `'No'` to `'Ever smoked at least 100 cigarettes?'` had their `NaN` values for `'Currently Smoke?'` logically filled with `'Not at all'`.
    * **Diabetes:** Participants who answered `'No'` to `'Ever told by doctor they had Diabetes?'` had their `NaN` values for `'Currently taking diabetic Pills?'` logically filled with `'No'`.
* **Numerical Imputation (Median):** Missing values in critical numerical columns were filled with the **median** value of that column to preserve the distribution while eliminating `NaN`s. This was applied to:
    * `Body Mass Index (BMI: kg/m^2)`
    * `Systolic Blood Pressure` & `Diastolic Blood Pressure`
    * `Ratio of family income to poverty`
    * `Weight (kg)` & `Standing Height (cm)`
    * `Glycohemoglobin (HbA1c) value (%)`
* **Categorical Imputation (New Class):** Remaining missing values in key categorical columns (like `Education`, `Ever told... Diabetes?`) were filled with a distinct `'Not provided'` category to handle missingness as separate information.
* **Column Removal:** Columns that were deemed redundant, too noisy, or outside the scope of this analysis (e.g., `Marital status`, `Average number of drinks`, `Age when first diagnosed`) were strategically dropped to create the final, clean dataset `df`.

### 1.2. Feature Engineering

New, high-level features were created from existing data to provide deeper insights and build more powerful models:

* **`AHA_Category`:** Blood pressure was classified into 5 categories (Normal, Elevated, Stage 1, Stage 2, Crisis) based on the **2017 AHA/ACC guidelines**.
* **`ESH_Category`:** Blood pressure was also classified (Optimal, Normal, High-Normal, Grade 1-3) based on the **2023 ESH guidelines**.
* **`Diabetes_Status`:** Participants were categorized into `Normal`, `Prediabetes`, or `Diabetes` based on their `HbA1c` levels, according to **ADA guidelines** (<5.7%, 5.7-6.4%, >=6.5%).

---

## 2. Analysis Plan (Exploratory Data Analysis)

This section outlines the detailed exploratory data analysis (EDA) to be performed on the final cleaned dataset (`df`).

### 2.1. Univariate Analysis (Understanding Each Variable)

* **Numerical Variables:**
    * Plot histograms and box plots for `Age`, `BMI`, `HbA1c`, `Systolic Blood Pressure`, and `Ratio of family income` to understand their distributions, identify skewness, and detect outliers.
* **Categorical Variables:**
    * Plot count plots (bar charts) to understand the frequency of each category for:
        * `Diabetes_Status` (Target Variable 1)
        * `AHA_Category` (Target Variable 2)
        * `Gender`
        * `Race/Hispanic origin category`
        * `Highest level of education attained`
        * `Currently Smoke?`
        * `Drank alcohol in the past 12 months?`

### 2.2. Bivariate & Multivariate Analysis (Finding Relationships)

This is the core of the analysis, exploring how variables interact.

#### 🎯 **Analysis 1: Risk Factors for Diabetes (`Diabetes_Status`)**

* **Q1: How do numerical factors affect diabetes risk?**
    * Analyze `Age` vs. `Diabetes_Status` (Box plot) to see how diabetes prevalence increases with age.
    * Analyze `BMI` vs. `Diabetes_Status` (Box plot) to quantify the link between obesity and diabetes.
    * Analyze `Ratio of family income to poverty` vs. `Diabetes_Status` (Violin plot) to explore socioeconomic impact.
* **Q2: How do categorical factors affect diabetes risk?**
    * Analyze `Race` vs. `Diabetes_Status` (Stacked bar chart) to identify disparities among different racial/ethnic groups.
    * Analyze `Education Level` vs. `Diabetes_Status` (Stacked bar chart) to see if education impacts diabetes rates.
    * Analyze `Smoking Status` vs. `Diabetes_Status` (Chi-square test, Stacked bar chart).
* **Q3: How does hypertension correlate with diabetes?**
    * Create a cross-tabulation (heatmap) between `Diabetes_Status` and `AHA_Category` to see the comorbidity (how many people have both).

#### 🎯 **Analysis 2: Risk Factors for Hypertension (`AHA_Category`)**

* **Q1: How do numerical factors affect hypertension risk?**
    * Analyze `Age` vs. `AHA_Category` (Box plot) to show the strong correlation between age and high blood pressure.
    * Analyze `BMI` vs. `AHA_Category` (Box plot) to confirm the link between weight and hypertension.
    * Analyze `HbA1c` vs. `AHA_Category` (Box plot) to explore the link from the other direction (how diabetic status affects BP).
* **Q2: How do categorical factors affect hypertension risk?**
    * Analyze `Race` vs. `AHA_Category` (Stacked bar chart) to check for disparities.
    * Analyze `Smoking Status` vs. `AHA_Category` (Stacked bar chart).
    * Analyze `Alcohol Use` vs. `AHA_Category` (Stacked bar chart).

---

## 3. Predictive Modeling Plan 

* **Model 1: Diabetes Prediction**
    * **Target:** `Diabetes_Status` (will be binarized: `Diabetes` vs. `Not Diabetes`).
    * **Features:** `Age`, `Gender`, `Race`, `Education`, `BMI`, `Systolic Blood Pressure`, `Smoking Status`, `Alcohol Use`, `Income Ratio`.
    * **Models:** Logistic Regression (for interpretability), Random Forest (for performance).
    * **Goal:** Identify the **Top 5 most important factors** that predict diabetes.

* **Model 2: Hypertension Prediction**
    * **Target:** `AHA_Category` (will be binarized: `Hypertension` vs. `Normal/Elevated`).
    * **Features:** `Age`, `Gender`, `Race`, `BMI`, `HbA1c`, `Smoking Status`, `Alcohol Use`, `Income Ratio`.
    * **Models:** Logistic Regression, XGBoost.
    * **Goal:** Build a high-accuracy model to screen for hypertension risk.

## 4. How to Use This Repository

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/asserelsheikh/NHANES-Health-Risk-Analysis.git](https://github.com/asserelsheikh/NHANES-Health-Risk-Analysis.git)
    ```
2.  **Install dependencies:**
    ```bash
    pip install pandas numpy matplotlib seaborn scikit-learn
    ```
3.  **Run the analysis:**
    * The `Data_Cleaning_NHANES.ipynb` notebook contains all the cleaning steps.
    * The `NHANES_cleaned.csv` file is the final dataset, ready for analysis.

## 5. License

This project is licensed under the **MIT License**.
---

## 🚀 How to Run
1. Clone this repository:
   ```bash
   git clone https://github.com/<username>/NHANES-Health-Risk-Analysis.git

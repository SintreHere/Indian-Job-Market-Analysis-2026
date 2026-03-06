# 🇮🇳 India Job Market — Salary Trends & Prediction 2026

> **Exploratory Data Analysis · Machine Learning · Salary Forecasting**  
> Uncovering what drives tech compensation across India's top cities, roles, and skill stacks.

---

## 📌 Overview

This repository contains a full end-to-end data science project on India's technology job market for 2026. Using a dataset of **10,000 curated job listings**, the project explores salary distributions, identifies skill-based pay premiums, and builds predictive models to forecast compensation based on role, experience, location, and technical skills.

The analysis was published as a professional report aimed at providing actionable insights for **job seekers, recruiters, and workforce planners**.

---

## 📁 Repository Structure

```
india-job-market-2026/
│
├── 📓 EDA.ipynb                                          # Full Jupyter Notebook — EDA + ML pipeline
├── 📊 India_Job_Market_Salary_Trends_2026_CLEAN.csv      # Cleaned dataset (10,000 records)
├── 📄 India_JobMarket_2026_Report.docx                   # Professional LinkedIn report (Word)
└── 📖 README.md                                          # You are here
```

---

## 📊 Dataset

**File:** `India_Job_Market_Salary_Trends_2026_CLEAN.csv`

| Column | Type | Description |
|---|---|---|
| `job_id` | int | Unique listing identifier |
| `job_title` | string | Role (ML Engineer, Data Scientist, DevOps, etc.) |
| `company_name` | string | Employer name (TCS, Infosys, Flipkart, etc.) |
| `location` | string | City (Bangalore, Mumbai, Delhi, Hyderabad…) |
| `experience_required_years` | int | Years of experience required |
| `skills` | string | Comma-separated required skill tags |
| `salary_min_inr` | int | Minimum salary offered (INR per annum) |
| `salary_max_inr` | int | Maximum salary offered (INR per annum) |
| `employment_type` | string | Full-time / Contract / Internship |
| `remote_option` | string | Onsite / Hybrid / Remote |

**Stats at a glance:**
- 🗂️ 10,000 rows · 10 columns · 0 nulls · 0 duplicates
- 💰 Salary range: ₹4.6 LPA — ₹25.9 LPA
- 📍 10+ major Indian cities
- 💼 9 job role categories

---

## 🔬 Notebook Walkthrough

**File:** `EDA.ipynb`

The notebook follows a structured pipeline:

### 1. 📥 Data Loading & Validation
- Load CSV, inspect shape and types
- Confirm zero nulls and zero duplicates
- Descriptive statistics on numeric columns

### 2. 📈 Salary Distribution Analysis
- Compute `salary_avg_lpa` as midpoint of min/max ranges converted to LPA
- Histogram + KDE plot (raw and log-transformed)
- Skewness check: **0.596** → raw target retained (below log-transform threshold)

### 3. 💼 Role & Location Analysis
- Boxplot: salary distribution by job title (sorted by median)
- Interactive bar chart: median salary by city (Plotly)
- Key finding: ML Engineers earn **~75% more** than Frontend Developers at the median

### 4. 📉 Experience vs. Salary
- Scatter plot coloured by role + OLS trendline
- Line chart: average salary per experience band, per role
- Pearson correlation: **0.004** (near-zero — role type dominates over tenure)

### 5. 🛠️ Employment Type & Remote Work
- Violin plots: salary distribution across Full-time / Contract / Internship
- Violin plots: Onsite / Hybrid / Remote comparison
- Remote carries a slight **−0.2 LPA** median discount

### 6. 🧠 Skill Premium Analysis
- Explode multi-skill strings into individual rows
- Median salary by skill (top 15, filtered to 50+ occurrences)
- Machine Learning, Kubernetes, and AWS top the skill-premium leaderboard

### 7. 🔧 Feature Engineering
- One-hot encode: `job_title`, `company_name`, `location`, `employment_type`, `remote_option`
- Binary skill flags for 12 key skills: Python, SQL, AWS, ML, Power BI, Tableau, Node.js, Azure, Docker, React, Java, Kubernetes
- Final matrix: **10,000 × 43**

### 8. 🤖 Model Building
- Train/test split: 80/20, `random_state=42`
- StandardScaler fitted on train only
- Models: Linear Regression, RidgeCV, LassoCV, RandomForestRegressor (200 trees), RF + GridSearchCV

### 9. 📊 Model Evaluation

| Model | MAE (LPA) | RMSE (LPA) | R² Score |
|---|---|---|---|
| **Lasso (CV)** ⭐ | 2.69 | 3.22 | **0.4506** |
| Linear Regression | 2.69 | 3.22 | 0.4501 |
| Ridge (CV) | 2.69 | 3.22 | 0.4501 |
| RF Tuned | 2.70 | 3.24 | 0.4445 |
| Random Forest (200) | 2.72 | 3.29 | 0.4276 |

> **Winner:** Lasso (CV) with best α=0.01, zeroing out 12/42 redundant features automatically.

### 10. 🔍 Feature Importance & SHAP
- Random Forest feature importances (top 20)
- SHAP TreeExplainer beeswarm plot (500-sample subset)
- Job title features + `skill_machine_learning` + `skill_kubernetes` are top drivers

### 11. 🔮 Salary Projections
- 210 future scenarios: 21 experience levels × 10 roles
- Baseline: Bangalore · TCS · Full-time · Onsite
- 95% confidence intervals from std across 300 RF estimators

---

## 📦 Dependencies

```bash
pip install pandas numpy matplotlib seaborn plotly scikit-learn shap feature-engine
```

Or install from requirements in one shot:

```bash
pip install pandas numpy matplotlib seaborn plotly scikit-learn shap feature-engine jupyter
```

---

## 🚀 How to Run

```bash
# Clone the repo
git clone https://github.com/your-username/india-job-market-2026.git
cd india-job-market-2026

# Install dependencies
pip install -r requirements.txt   # or use the pip install block above

# Launch the notebook
jupyter notebook EDA.ipynb
```

> 💡 If running on **Google Colab**, skip the file upload cell and load the CSV directly:
> ```python
> df = pd.read_csv('India_Job_Market_Salary_Trends_2026_CLEAN.csv')
> ```

---

## 💡 Key Findings

| Finding | Detail |
|---|---|
| 🏆 Top-paying role | ML Engineer — ₹17.54 LPA median |
| 📍 Top-paying city | Bangalore (highest consistent median) |
| 🛠️ Top-paying skill | Machine Learning |
| 📉 Experience correlation | ~0.004 (role type dominates over years of tenure) |
| 🏅 Best ML model | Lasso CV — R² 0.4506, MAE ₹2.69 LPA |
| 🔮 Fastest growing stage | 3–7 years experience — steepest salary acceleration |

---

## 📄 Report

A fully formatted professional report (`India_JobMarket_2026_Report.docx`) is included, featuring:

- Executive summary with KPI cards
- Role-by-role salary breakdown tables
- Skill premium analysis
- Model leaderboard
- Career trajectory projections
- Strategic takeaways for job seekers, recruiters, and policymakers

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python)
![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?logo=pandas)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-1.x-F7931E?logo=scikit-learn)
![Plotly](https://img.shields.io/badge/Plotly-5.x-3F4F75?logo=plotly)
![SHAP](https://img.shields.io/badge/SHAP-Explainability-blueviolet)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter)

---

## 📬 Connect

Found this analysis useful? Let's connect on LinkedIn!  
⭐ Star this repo if it helped you understand India's 2026 tech salary landscape.

---

*Analysis conducted for educational and career insight purposes. Dataset reflects synthetic job market trends for 2026.*

# 🦠 COVID-19 Global Case Study

![Python](https://img.shields.io/badge/Python-3.x-blue?style=flat&logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-green?style=flat&logo=pandas)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-orange?style=flat)
![NumPy](https://img.shields.io/badge/NumPy-Numerical-yellow?style=flat&logo=numpy)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-red?style=flat&logo=jupyter)

A comprehensive Python-based data analysis of COVID-19 confirmed cases, deaths, and recoveries across 276 countries and regions — January 2020 to May 2021.

---

## 📖 Overview

The COVID-19 pandemic, caused by the SARS-CoV-2 virus, emerged in late 2019 and rapidly spread globally, leading to significant health, economic, and social impacts. This case study demonstrates the crucial role of data analysis in managing such pandemics — using Python to track confirmed cases, recoveries, and deaths and derive actionable insights for policymakers.

---

## 🎯 Objectives

- **Practical Python** — Apply Pandas, Matplotlib, and NumPy on real-world pandemic data.
- **Insightful Analysis** — Understand spread, recovery, and mortality dynamics across regions.
- **Skill Development** — Build confidence handling messy, real-world time-series datasets.

---

## 📂 Datasets

Three datasets from the JHU CSSE COVID-19 repository, each with **276 rows** and **498 columns** (Province/State, Country/Region, Lat, Long + daily date columns):

| Dataset | Description |
|---|---|
| `covid_19_confirmed_v1` | Cumulative confirmed cases per day per region |
| `covid_19_deaths_v1` | Cumulative deaths per day per region |
| `covid_19_recovered_v1` | Cumulative recoveries per day per region |

> **Data spans:** 22 Jan 2020 → 29 May 2021

---

## ⚠️ Known Issue — Mixed Date Formats

The dataset contains **two date formats**:
- `M/D/YY` format — e.g. `1/22/20` (early columns)
- `D/M/YYYY` format — e.g. `1/2/2020` which means **Feb 1**, not Jan 2

When reading from XLSX, Excel pre-parses the latter incorrectly, producing zigzag graphs.

**Fix for CSV:**
```python
df["Date"] = pd.to_datetime(df["Date"], dayfirst=True)
```

**Fix for XLSX:**
```python
import datetime

def fix_col_dates(df):
    new_cols = {}
    for col in df.columns:
        if isinstance(col, datetime.datetime):
            new_cols[col] = pd.Timestamp(year=col.year, month=col.day, day=col.month)
        else:
            new_cols[col] = col
    return df.rename(columns=new_cols)

df_c = fix_col_dates(df_c)
df_d = fix_col_dates(df_d)
df_r = fix_col_dates(df_r)
```

---

## 📊 Analysis Questions Covered

### 1. Data Loading
- Load all 3 datasets into Pandas DataFrames from CSV / XLSX.

### 2. Data Exploration
- Shape, dtypes, and structure of each dataset.
- Plot confirmed cases over time for top countries.
- Plot confirmed cases over time for China.

### 3. Handling Missing Data
- Identify null values and apply **forward-fill** imputation for time-series data.

### 4. Data Cleaning & Preparation
- Replace blank `Province/State` values with `"All Provinces"`.

### 5. Independent Dataset Analysis
- Peak daily new cases in **Germany, France, and Italy** — which had the highest single-day surge and when?
- Recovery rates (recoveries / confirmed) for **Canada vs Australia** as of Dec 31, 2020.
- Death rate distribution across **Canadian provinces** — highest and lowest province identified.

### 6. Data Transformation
- Transform deaths dataset from **wide format → long format**.
- Total deaths reported per country.
- Top 5 countries by highest average daily deaths.
- Evolution of total deaths over time in the **United States**.

### 7. Data Merging
- Merge confirmed, deaths, and recovered datasets on `Country/Region` and `Date`.
- Monthly sum of all three metrics for global and country-level analysis.
- Focused analysis for **United States, Italy, and Brazil**.

### 8. Combined Data Analysis
- Top 3 countries with highest average **death rates** throughout 2020.
- Total recoveries vs deaths in **South Africa**.
- Monthly **recovery-to-confirmed ratio** for the US from March 2020 to May 2021.

---

## 🛠️ Key Techniques Used

### Data Wrangling
```python
df.fillna(method='ffill')              # Forward-fill for time-series nulls
df["Province/State"].fillna("All Provinces")  # Clean blank province values
pd.melt()                              # Wide → long format transformation
df.groupby("Country/Region").sum()     # Aggregate province-level data
```

### Analysis
```python
df.diff()                              # Daily new cases from cumulative totals
recovery_rate = recovered / confirmed  # Recovery rate metric
death_rate = deaths / confirmed        # Death rate metric
pd.merge(df1, df2, on=["Country/Region", "Date"])  # Join all 3 datasets
df["Date"].dt.month_name()             # Monthly grouping
```

### Visualisation
- Time-series line plots for top 3 countries (US, India, Brazil)
- Country-level plots for China, US deaths, Germany/France/Italy peaks
- Monthly bar charts for pandemic progression

---

## 📁 Project Structure

```
📁 covid-19-case-study/
  ├── 📓 Covid-19_Project.ipynb
  ├── 📊 covid_19_dataset.xlsx
  │     ├── covid_19_confirmed_v1
  │     ├── covid_19_deaths_v1
  │     └── covid_19_recovered_v1
  └── 📄 README.md
```

---

## ▶️ How to Run

```bash
git clone https://github.com/your-username/covid-19-case-study
cd covid-19-case-study
pip install pandas matplotlib numpy openpyxl
jupyter notebook Covid-19_Project.ipynb
```

# 🏠 House Price Prediction
### Data Analytics Internship — ApexPlanet Software Pvt. Ltd.

![Python](https://img.shields.io/badge/Python-3.10+-blue?style=flat&logo=python)
![Pandas](https://img.shields.io/badge/Pandas-2.0+-green?style=flat&logo=pandas)
![Status](https://img.shields.io/badge/Week--1-Completed-brightgreen?style=flat)
![Internship](https://img.shields.io/badge/ApexPlanet-Internship-orange?style=flat)

---

## 📌 About This Project

This repository documents **Week 1 – Task 1** of the Data Analytics Internship at **ApexPlanet Software Pvt. Ltd.**

The goal is to master the foundational step of any data analytics pipeline — **Data Immersion & Wrangling** — using a House Price Prediction dataset. This involves acquiring, understanding, cleaning, and transforming raw data into an analysis-ready format.

> **Timeline:** 10 Days | **Task:** Data Immersion & Wrangling

---

## 🎯 Objective

To rapidly acquaint with the provided dataset and master the critical first step of any analysis:
- Acquiring and exploring the dataset
- Cleaning and resolving data quality issues
- Preparing a final, analysis-ready dataset

---

## 📁 Repository Structure

```
House_Price_Prediction/
│
├── 📂 data/
│   ├── raw_dataset.csv          # Original, unmodified dataset
│   └── cleaned_dataset.csv      # Final analysis-ready dataset
│
├── 📂 notebooks/
│   └── data_wrangling.ipynb     # Jupyter Notebook with full EDA & cleaning
│
├── 📂 scripts/
│   └── cleaning_script.py       # Standalone Python cleaning script
│
├── 📂 docs/
│   └── data_dictionary.md       # Data dictionary documenting all variables
│
└── README.md                    # Project overview (this file)
```

---

## 📋 Task Breakdown

### Step 1 — Data Access & Familiarization

- Loaded the House Price dataset and performed an initial exploration
- Created a **Data Dictionary** documenting each variable's:
  - Name & Data Type
  - Example Values
  - Business / Domain Relevance

| Column Name | Data Type | Example | Description |
|---|---|---|---|
| `Id` | Integer | 1, 2, 3 | Unique identifier for each house |
| `LotArea` | Integer | 8450 | Lot size in square feet |
| `Neighborhood` | String | CollgCr | Physical location within city |
| `YearBuilt` | Integer | 2003 | Original construction year |
| `OverallQual` | Integer | 7 | Overall material and finish quality (1–10) |
| `TotalBsmtSF` | Float | 856.0 | Total square feet of basement area |
| `GrLivArea` | Integer | 1710 | Above-grade living area in sq. ft. |
| `BedroomAbvGr` | Integer | 3 | Number of bedrooms above basement |
| `GarageCars` | Float | 2.0 | Garage capacity in car units |
| `SalePrice` | Integer | 208500 | Target variable — property sale price (USD) |

> 📄 Full data dictionary available in [`docs/data_dictionary.md`](docs/data_dictionary.md)

---

### Step 2 — Data Quality Assessment

Initial profiling revealed the following issues:

| Issue | Columns Affected | Count |
|---|---|---|
| Missing Values | `LotFrontage`, `GarageType`, `MasVnrArea` & more | ~19 columns |
| Duplicate Rows | — | 0 |
| Inconsistent Formatting | `MoSold` (numeric month vs. name) | 1 column |
| Outliers Detected | `GrLivArea`, `LotArea`, `SalePrice` | 3 columns |
| Incorrect Data Types | `MSSubClass` stored as int, should be categorical | 1 column |

**Quick profiling code used:**
```python
import pandas as pd

df = pd.read_csv('data/raw_dataset.csv')

print(f"Shape: {df.shape}")
print(f"\nMissing Values:\n{df.isnull().sum()[df.isnull().sum() > 0]}")
print(f"\nDuplicates: {df.duplicated().sum()}")
print(f"\nData Types:\n{df.dtypes}")
print(f"\nStatistical Summary:\n{df.describe()}")
```

---

### Step 3 — Data Cleaning & Transformation

The following cleaning operations were applied:

#### 🔧 Handling Missing Values
```python
# Numerical columns — fill with median
num_cols = ['LotFrontage', 'MasVnrArea', 'GarageYrBlt']
for col in num_cols:
    df[col].fillna(df[col].median(), inplace=True)

# Categorical columns — fill with 'None' (no feature present)
cat_cols = ['Alley', 'BsmtQual', 'FireplaceQu', 'GarageType', 'PoolQC', 'Fence']
for col in cat_cols:
    df[col].fillna('None', inplace=True)
```

#### 🔧 Removing Outliers
```python
# Remove extreme outliers in GrLivArea (above 4000 sqft with low price)
df = df[~((df['GrLivArea'] > 4000) & (df['SalePrice'] < 200000))]
```

#### 🔧 Feature Engineering
```python
# Age of house at time of sale
df['HouseAge'] = df['YrSold'] - df['YearBuilt']

# Total bathrooms
df['TotalBath'] = (df['FullBath'] + (0.5 * df['HalfBath']) +
                   df['BsmtFullBath'] + (0.5 * df['BsmtHalfBath']))

# Total square footage
df['TotalSF'] = df['TotalBsmtSF'] + df['1stFlrSF'] + df['2ndFlrSF']
```

#### 🔧 Type Correction & Standardization
```python
# Convert MSSubClass to categorical
df['MSSubClass'] = df['MSSubClass'].astype(str)

# Standardize date columns
df['YearBuilt'] = pd.to_datetime(df['YearBuilt'], format='%Y')
```

#### 🔧 Save Final Dataset
```python
df.to_csv('data/cleaned_dataset.csv', index=False)
print(f"Clean dataset saved! Shape: {df.shape}")
```

---

## 📦 Deliverables

| Deliverable | Status | Location |
|---|---|---|
| ✅ Data Dictionary | Completed | `docs/data_dictionary.md` |
| ✅ Cleaning Script | Completed | `scripts/cleaning_script.py` |
| ✅ Cleaned Dataset | Completed | `data/cleaned_dataset.csv` |
| ✅ Jupyter Notebook | Completed | `notebooks/data_wrangling.ipynb` |
| 🎥 LinkedIn Video | In Progress | 3-5 min walkthrough |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Python 3.10+ | Core programming language |
| Pandas | Data manipulation & cleaning |
| NumPy | Numerical operations |
| Matplotlib / Seaborn | Data visualization & outlier detection |
| Jupyter Notebook | Interactive exploration |
| GitHub | Version control & deliverable submission |

---

## 🚀 How to Run

```bash
# 1. Clone the repository
git clone https://github.com/your-username/House_Price_Prediction.git
cd House_Price_Prediction

# 2. Install dependencies
pip install pandas numpy matplotlib seaborn jupyter

# 3. Run the cleaning script
python scripts/cleaning_script.py

# 4. Or open the notebook
jupyter notebook notebooks/data_wrangling.ipynb
```

---

## 📊 Key Findings

- The dataset had **1,460 rows** and **81 columns** before cleaning
- **19 columns** had missing values — most were structural (e.g., no garage = missing GarageType)
- **2 significant outliers** were removed from `GrLivArea`
- **3 new features** were engineered: `HouseAge`, `TotalBath`, `TotalSF`
- Final cleaned dataset: **1,458 rows × 84 columns**

---

## 👤 Author

**[Vivek Kumar Tiwari]**
Data Analytics Intern — ApexPlanet Software Pvt. Ltd.
📧 vkt.vivek007@gmail.com
🔗 [LinkedIn](https://linkedin.com/in/yourprofile) | [GitHub](https://github.com/Virvivek007/)

---

## 🏢 Organization

**ApexPlanet Software Pvt. Ltd.**
🌐 [www.apexplanet.in](https://www.apexplanet.in)

---

*This project is part of a structured Data Analytics Internship program. Week 1 focuses on data foundations — the backbone of every successful analytics project.*

# Titanic Survival Analysis — Capstone Project Report

**Student:** Eyas Alghamdi  
**Dataset:** Titanic Passenger Manifesto (`titanic.csv`)  
**Target Variable:** `survived` (0 = No, 1 = Yes)

---

## 1. Introduction

This project analyses approximately 891 records of passengers aboard the RMS Titanic. The dataset covers demographics (age, sex), socio-economic indicators (class, fare), and travel details (embarkation point, family relations) from the 1912 voyage.

The central question driving the analysis is:

> **What demographic and socio-economic factors most strongly influenced a passenger's probability of survival?**

Secondary questions explored include:
- How did survival rates differ between genders (the "women and children first" rule)?
- To what extent did passenger class (1st, 2nd, 3rd) determine access to lifeboats?
- Is there a measurable relationship between family size and survival?

All work was conducted in Python using Jupyter Notebooks, utilizing Pandas, NumPy, Matplotlib, Seaborn, and Scikit-learn.

---

## 2. Cleaning Summary

### Problems Found

| Column | Issue | Action Taken |
|---|---|---|
| `age` | 177 missing values (~20%) | Filled with median to preserve distribution |
| `embarked` | 2 missing values | Filled with mode ('S') |
| `deck` | ~77% missing | Dropped — column lacks statistical significance |
| `fare` | Extreme values (up to $512) | Capped at 99th percentile (~$250) |
| `sex` | Stored as string | Encoded for numerical analysis |
| `pclass` | Stored as int | Treated as categorical feature |

### Decisions Explained

- **Median Imputation for `age`:** Age data was missing for nearly 20% of passengers. Using the median instead of the mean prevented the average from being skewed by outliers (very old/young passengers), maintaining a more realistic profile of the middle-aged traveler.
- **Outlier capping at 99th percentile:** A handful of passengers paid exceptionally high fares (over $500). Capping these at the 99th percentile prevents these "luxury outliers" from distorting the average fare calculations while keeping the data points in the set.

---

## 3. Feature Engineering Summary

Four categories of features were engineered to improve predictive clarity:

| Feature | Type | Rationale |
|---|---|---|
| `Age_scaled` | Standard Scaled | Normalizes age to a mean of 0 for fair comparison |
| `Fare_scaled` | Standard Scaled | Normalizes financial data to match other features |
| `FamilySize` | Discrete | `sibsp` + `parch` + 1; captures total family unit |
| `Age_Class_Interaction`| Interaction | Age × Pclass; tests if age matters more in 1st class |
| `IsAlone` | Binary | 1 if FamilySize is 1; identifies solo travelers |

**Redundancy removal:** High-cardinality or redundant columns like `Name`, `Ticket`, and `Cabin` were excluded from the primary feature set to reduce noise and prevent over-fitting.

---

## 4. Key Findings

### Finding 1 — Gender is the single strongest predictor of survival

Sorting survival rates by gender reveals an undeniable protocol during the disaster:

| Gender | Survival Rate |
|---|---|
| Female | ~74.2% |
| Male | ~18.9% |

Females were nearly **4 times more likely** to survive than males. This confirms that the "Women and Children First" maritime policy was the primary filter for lifeboat access.

### Finding 2 — Socio-economic class dictates survival probability

The "Wealth Gap" on the Titanic was a matter of life and death:

| Class | Survival Rate |
|---|---|
| 1st Class | ~62.9% |
| 2nd Class | ~47.3% |
| 3rd Class | ~24.2% |

1st Class passengers were **2.6x more likely** to survive than those in 3rd Class. This suggests that physical proximity to the boat deck and social status were key survival drivers.

### Finding 3 — Family size correlates with survival, but only for small groups

The analysis confirms that traveling alone was a disadvantage. Solo travelers had a survival rate of only **30%**. However, small families (2-4 people) saw a survival "lift" to **~58%**, likely due to mutual assistance. Survival dropped again for large families (5+), as keeping large groups together in a panic is significantly more difficult.

**The Math Basics notebook (04_math.ipynb) confirmed:**
- Mean Age: ~29.7 | Std Dev: ~13.0 (Manual NumPy calculation matches library functions).
- Cosine Similarity between a 1st class survivor and 3rd class non-survivor = **0.12** — mathematically proving these groups are almost entirely distinct.
- **P(Survival > 0.5 | Female and 1st Class) = 96.8%** — proving that this specific demographic was almost guaranteed to survive.

---

## 5. Next Steps

If more time were available, the following steps would strengthen the analysis:

1. **Title Extraction:** Extracting titles (Mr., Mrs., Master., Dr.) from the `Name` column to identify professional status or social rank (e.g., Doctors or Nobility).
2. **Predictive Modeling:** Applying Random Forest or Logistic Regression to create a model that predicts survival for any new passenger input.
3. **Embarkation Influence:** Analyzing why passengers from Cherbourg ('C') had a higher survival rate than those from Southampton ('S').

---

*Report prepared by Eyas Alghamdi - github link: https://github.com/SchoolAccountLolXd/tuwaiq-assignment.git*

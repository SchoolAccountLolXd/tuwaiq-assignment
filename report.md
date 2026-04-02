# Titanic Passenger Survival Analysis
### ML Foundations Capstone Project — Written Report
**Author:** Student | **Dataset:** Titanic | **Date:** 2024

---

## 1. Introduction

For this project I chose the **Titanic passenger dataset** — one of the most well-documented
real-world datasets in data science. It contains records for **891 passengers** (after deduplication)
with information on their age, sex, ticket class, fare paid, family members aboard, port of
embarkation, and whether they survived the disaster.

The central questions I tried to answer were:

- **Who survived, and who did not?** Can we find clear patterns in the data?
- **Did wealth (proxied by fare and class) predict survival?**
- **Did demographics (age, sex, family size) play a role?**
- **Which features are most informative for understanding survival?**

All analysis was performed in Python using Jupyter Notebooks. The project is split into three
phases: cleaning, feature engineering, and exploratory analysis.

---

## 2. Cleaning Summary

The raw dataset had several problems that needed fixing before any meaningful analysis could begin.

**Data type errors:** Two columns were stored as plain integers when they should not be.
`survived` (a binary 0/1 label) and `pclass` (a class label with values 1, 2, 3) were both
converted to the `category` dtype. This prevents accidental arithmetic on label columns and makes
groupby operations cleaner.

**Missing values:** Three columns had missing data:

- **`age`** had 177 missing values (~19.8%). I filled these with the **median age** rather than
  the mean, because the age distribution is slightly skewed and the median is more robust to
  extreme values. Using the mean would slightly inflate imputed ages.

- **`embarked`** had just 2 missing values. I filled these with the **mode** (Southampton, 'S'),
  which is the port where the vast majority of passengers boarded. With only 2 missing, any
  imputation strategy produces nearly identical results.

- **`cabin`** had 687 missing values — over 77% of the column. Rather than guess at cabin
  assignments for most of the dataset, I **dropped this column entirely**. Imputing a feature
  when three-quarters of values are missing would introduce more noise than signal.

**Duplicate rows:** The raw file contained **3 exact duplicate rows** (added deliberately for
cleaning practice). These were detected using `.duplicated()` and removed.

**Outlier treatment:** The `fare` column contained extreme values in a long right tail. Using the
IQR method I identified that values above approximately £65 were statistical outliers. Rather than
deleting these passengers, I **capped fare at the 99th percentile**, preserving all records while
limiting the distorting influence of extreme values.

---

## 3. Feature Engineering Summary

After cleaning, I created eight new features to make the data richer and more useful:

| Feature | Type | Why It Was Created |
|---------|------|--------------------|
| `sex_male` | One-hot | Converts sex to a binary number models can use |
| `embarked_Q`, `embarked_S` | One-hot | Converts embarkation port to binary columns |
| `pclass_ordinal` | Ordinal | Remaps class 1→3, 2→2, 3→1 so higher = better |
| `age_scaled` | Scaled | Normalises age to mean=0, std=1 for model use |
| `fare_scaled` | Scaled | Normalises fare to mean=0, std=1 for model use |
| `fare_per_class_unit` | Ratio | Fare ÷ class ordinal; shows relative spending within tier |
| `family_size` | Domain | sibsp + parch + 1; captures travel group size |
| `class_fare_interaction` | Interaction | pclass_ordinal × fare_scaled; composite wealth signal |
| `fare_log` | Log transform | log(1+fare); reduces heavy right skew in fare |
| `age_group` | Binned | Groups age into Child / Youth / Adult / Middle-Aged / Senior |

The most valuable of these turned out to be `family_size` and `class_fare_interaction`.
`family_size` captures the evacuation dynamics of groups, and the interaction term proved to
have a strong correlation with survival — stronger than fare or class alone — validating the
hypothesis that *combined* wealth signals matter more than either component separately.

---

## 4. Key Findings

### Finding 1: Passenger Class Was the Single Strongest Predictor of Survival

First-class passengers survived at roughly **60–63% rate**, compared to approximately **24%** for
third-class passengers. This is not merely a statistical correlation — it reflects real physical
and social conditions. First-class cabins were located on upper decks, giving passengers faster
access to the lifeboat deck. Historical records also indicate that third-class passengers
encountered locked gates and longer, more confusing routes to safety.

The chart below (from Section 3.9 in the EDA notebook) makes this gap undeniable:

> *See `data/cleaned/groupby_class_survival.png`*

### Finding 2: Fare Was a Powerful Proxy for Survival Chances

Even setting class aside, passengers who paid higher fares survived at significantly higher rates.
The boxplot comparing fare distributions for survivors vs non-survivors (Section 3.5) shows that
survivors had a median fare roughly 3× higher than those who did not survive. The log-transformed
fare distribution also reveals that nearly all the high-paying passengers were survivors.

When we computed conditional probability in Section 3.13:
**P(survived | 1st class, paid above median fare) ≈ 60%**, compared to an overall survival
rate of ~38% across all passengers. This 22-percentage-point gap quantifies the survival
advantage of wealth.

### Finding 3: Most Passengers Travelled Alone, and Family Size Had a Non-Linear Effect

Over 60% of passengers were solo travellers (family_size = 1). Interestingly, small families
(2–4 members) had slightly better survival rates than solo travellers in some groups, possibly
because they could assist each other and were more likely to include women and children — groups
prioritised for lifeboats. However, very large families (6–8 members) had poor outcomes, likely
because coordinating a large group evacuation in chaos was extremely difficult.

---

## 5. What I Would Do Next

Given more time, I would take the following steps:

1. **Build a predictive model.** With clean features and a binary target (`survived`), a logistic
   regression or random forest model is the natural next step. I would evaluate it with
   cross-validation and measure precision, recall, and AUC-ROC.

2. **Analyse sex as a variable more deeply.** Historical accounts emphasise that women and
   children were prioritised for lifeboats ("women and children first"). The one-hot `sex_male`
   feature shows a strong correlation with survival — a more detailed breakdown by sex × class
   would reveal whether this policy was applied consistently across all three classes.

3. **Engineer cabin deck from the cabin column.** Even though `cabin` was mostly missing, the
   first letter of the cabin number indicates the deck (A, B, C, D, E, F, G). For the 200 or so
   passengers who do have cabin data, deck proximity to the top of the ship may be a strong
   predictor worth studying.

4. **Explore name-based features.** The passenger names contain titles (Mr., Mrs., Miss., Dr.,
   Rev., etc.) which reveal social status and can serve as a proxy for age, marital status, and
   gender in cases where those fields are missing.

5. **Produce an interactive dashboard** using Matplotlib subplots or Plotly to allow dynamic
   exploration of survival rates by any combination of class, sex, and age group simultaneously.

---

*This report was written to accompany the Jupyter notebooks submitted as part of the ML Foundations Capstone Project.*

# Titanic Survival Analysis: Comprehensive Executive Report

## **Lead Analyst:** Eyas Alghamdi
## **Date:** April 2026
## **Status:** Final Capstone Submission

---

### **1. Introduction and Research Scope**
The sinking of the RMS Titanic remains one of history’s most significant maritime disasters. Beyond the tragedy, it provides a unique laboratory for data science and predictive modeling. This project aims to analyze the passenger manifesto to answer a fundamental question: **Was survival random, or was it dictated by measurable social and economic factors?**

To answer this, I utilized a comprehensive four-phase pipeline:
1. **Phase 1 (Cleaning):** Removing noise and fixing missing data.
2. **Phase 2 (Engineering):** Creating new metrics like Family Size.
3. **Phase 3 (EDA):** Visualizing the hidden patterns of the disaster.
4. **Phase 4 (Mathematics):** Verifying the data using manual statistical formulas.

**Technical Sources:** To ensure the highest level of accuracy, I cross-referenced my Python logic with five industry-standard sources: **W3Schools** (Pandas syntax), **Stack Overflow** (Path debugging), **Pandas Official Docs**, **GeeksforGeeks** (Feature Scaling), and **Towards Data Science** (EDA Strategy).

---

### **2. Methodology: Data Hygiene and Integrity**

#### **2.1 Raw vs. Cleaned Data**
As shown in `01_cleaning.ipynb`, the raw data was "dirty." It contained 177 missing Age records and 2 missing Embarkation points. The `deck` column was missing over 70% of its data, making it statistically unreliable.

* **Action: Data Reduction.** I dropped the `deck` column entirely. In data science, using a column with 70% missing values leads to "hallucinated" results. By removing it, I ensured the model focuses only on high-quality information.
* **Action: Median Imputation.** For `age`, I used **Median Imputation**. I chose the median over the mean because the mean is easily skewed by extreme age values (very old or very young), whereas the median represents the true "middle" of the population. This preserved the distribution without shifting the average unnaturally.

#### **2.2 Managing Extreme Outliers**
While most passengers paid between $10 and $50 for a ticket, a few "luxury" tickets cost over $500. These outliers can pull the average survival statistics in the wrong direction, creating a false sense of what a "typical" passenger experienced. 

I implemented a **99th Percentile Cap**. This technique caps the highest values at the 99% mark, ensuring that the ultra-wealthy 1% do not distract from the trends affecting the other 99% of passengers.

---

### **3. Advanced Feature Engineering**

Following the **Day 15: Feature Scaling** module, I transformed the raw data into "Machine Learning Ready" features to improve model clarity.

#### **3.1 Standardization (Z-Score Scaling)**
Using the `StandardScaler`, I centered the `age` and `fare` around a mean of 0 with a standard deviation of 1. This allows us to compare a "high fare" to an "old age" on a fair mathematical scale. Without this, the larger numbers in the `fare` column would outweigh the `age` column simply because the numbers are bigger.

#### **3.2 The Interaction Term**
I created a custom feature: `Age_Class_Interaction`. This combines a passenger's age and their social class. 
* **Hypothesis:** Does age matter more if you are rich? 
* **Result:** Analysis showed that "Young 1st Class" passengers were saved significantly more often than "Young 3rd Class" passengers, proving that age was not the only factor; class provided the "access" to safety.

---

### **4. Exploratory Analysis and Core Findings**

The visualizations created in `03_eda.ipynb` reveal three undeniable truths about the Titanic disaster:

#### **A. The Social Protocol of 1912**
**Gender** was the single most powerful predictor of life. By plotting survival rates, we see that being female increased survival chances by nearly **400%** compared to males. This confirms that the "Women and Children First" maritime protocol was strictly enforced by the crew during the evacuation.

#### **B. The Wealth Safety Net**
The Boxplots created in Phase 3 show that survivors generally paid a much higher fare than non-survivors. **1st Class passengers** were located on the upper decks, closer to the boat deck. This physical proximity, combined with their social status, gave them a massive advantage in reaching lifeboats before they were full.

#### **C. The Family Factor**
My engineered feature, `FamilySize`, showed that passengers traveling in small groups (2–3 people) survived more often than those traveling alone. It suggests that family units helped each other navigate the chaos, while those alone were more likely to be overlooked or unable to find a clear path to safety.

---

### **5. Mathematical Validation and Statistical Discussion**

Based on **Day 45: Math-For-ML**, I didn't just trust the software libraries; I verified the numbers manually using NumPy.

* **Manual Standard Deviation:** I manually calculated the variance and standard deviation of passenger ages. This confirmed that our `StandardScaler` output was 100% accurate, ensuring that our transformed data was not corrupted during the coding process.
* **Cosine Similarity Analysis:** I calculated the "Vector Similarity" between the "average survivor" and the "average non-survivor." 
    * **Mathematical Proof:** The resulting similarity score was significantly low. In mathematical terms, this proves that the two groups were "orthogonal" or distinct. They did not share the same characteristics, proving that survival was **systematic**, not random.

---

### **6. Conclusion**

The analysis concludes that survival on the Titanic was not a matter of luck or "being in the right place at the right time." It was a highly structured outcome determined by **Gender, Wealth (Class), and Age.** By cleaning the data, engineering new features, and validating the results through mathematics, this project provides a clear, data-driven window into the social hierarchy of the early 20th century.

**AI Disclosure:** AI assistance (**Gemini / ChatGPT**) was used for code structuring, debugging complex Python errors, and explaining foundational math concepts. All analytical decisions, data cleaning strategies, and final explanations are the student's own work.

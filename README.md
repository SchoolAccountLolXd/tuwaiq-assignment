# Titanic Survival Analysis — Capstone Project

## **Student:** Eyas Alghamdi
## **Dataset:** Titanic Passenger Manifesto (Seaborn Library)

---

### **Project Overview**
This project builds a complete, end-to-end data analysis pipeline on the Titanic dataset. It covers **Data Cleaning**, **Feature Engineering**, **Exploratory Data Analysis (EDA)**, and **Foundational Mathematics**. The goal is to determine the primary socio-economic and demographic factors that dictated survival during the 1912 disaster.

---

### **Key Results & Insights**
* **Strongest Predictor:** **Gender** (Females had a ~74% survival rate vs. ~19% for males).
* **Economic Advantage:** **Ticket Fare and Class** (1st Class passengers had significantly higher survival probabilities).
* **Data Hygiene:** **177 missing Age values** were identified and filled using the **median** to maintain statistical integrity.
* **Outlier Control:** **Fare prices** were capped at the **99th percentile** to prevent luxury suite costs from skewing the average analysis.

---

### **Technical Stack & Methodology**
* **Cleaning:** Handled missing values (Imputation) and dropped sparse columns (Deck).
* **Engineering:** Created new metrics like **FamilySize** and **Age_Class_Interaction**.
* **EDA:** Utilized **Seaborn** and **Matplotlib** for high-impact visual storytelling.
* **Math:** Manually verified **Z-scores** and **Cosine Similarity** using **NumPy**.

---

### **Research Sources**
To ensure industry-standard code quality, the following sources were utilized:
1.  **W3Schools:** Pandas syntax and basic Python logic.
2.  **Stack Overflow:** Debugging data paths and URL integration.
3.  **Pandas Official Docs:** Advanced DataFrame manipulations.
4.  **GeeksforGeeks:** Mathematical scaling and feature engineering tutorials.
5.  **Towards Data Science:** Professional EDA layouts and Titanic benchmarks.

---

### **Project Notes**
* **Reproducibility:** All notebooks use **raw GitHub URLs**—the project can be run from any machine via Google Colab or VS Code.
* **Visual Proof:** All charts and data tables are rendered directly within the `.ipynb` files for immediate review.
* **AI Disclosure:** AI assistance (**Gemini / ChatGPT**) was used for code structuring, debugging, and explaining complex math. All analytical decisions and final explanations are the student's own work.


### **1. Problem Definition**

**Problem:**  
*Predicting the likelihood of employment within 6 months after graduation for university students.*

**3 Objectives:**  
1. Identify at-risk students early to provide career support.  
2. Help universities improve placement rates and curriculum.  
3. Assist students in making informed major/career choices.  

**2 Stakeholders:**  
1. University Career Services
2. Students & Parents

**1 KPI:**  
Accuracy of employment prediction within ±10% of actual placement rate per cohort
---

### **2. Data Collection & Preprocessing**

**2 Data Sources:**  
1. University Records
2. Alumni Survey & LinkedIn API

**Potential Bias:**  
**Selection bias** – Only surveying alumni who respond and active LinkedIn users  → overestimates employment for tech-savvy majors.  

**3 Preprocessing Steps:**  
1. **Handle missing data** → Impute GPA with median per major; drop rows with missing employment status.  
2. **Encode categorical features** → One-hot encode *major*, *gender*; label encode *job sector*.  
3. **Normalize numerical features** → Scale GPA, internship count, and attendance rate to [0,1] using scaler.  

---

### **3. Model Development**

**Chosen Model:** **XGBoost Classifier**  
**Justification:** Handles mixed data types, captures non-linear relationships, robust to outliers, and provides feature importance (e.g., GPA vs internships).  

**Data Split:**  
- **70% Training** – to learn patterns  
- **15% Validation** – for hyperparameter tuning  
- **15% Test** – final unbiased evaluation  

**2 Hyperparameters to Tune:**  
1. **`max_depth`** – Controls tree complexity; prevents overfitting.  
2. **`learning_rate`** – Smaller values improve generalization with more trees.  

---

### **4. Evaluation & Deployment**

**2 Evaluation Metrics:**  
1. **F1-Score** → Balances precision/recall; critical when employed class is imbalanced (e.g., 80% employed).  
2. **AUC-ROC** → Measures ranking ability across probability thresholds; robust to class imbalance.  

**Concept Drift:**  
Concept drift occurs when the statistical properties of the target variable (or the relationship between input features and target) change over time, making a trained model less accurate on new data.


**How to Monitor Post-Deployment:**  
- Track **model accuracy on new monthly cohorts**.  
- Use **Kolmogorov-Smirnov test** to detect feature distribution shifts (e.g., GPA trends).  
- Retrain if **performance drops >5%** for 2 consecutive months.  

**1 Technical Challenge in Deployment:**  
**Scalability** – Predicting for 10,000+ new graduates annually requires batch processing and low-latency inference.  
**Solution:** Deploy model via **FastAPI + Docker** on cloud (e.g., AWS SageMaker), with **daily batch predictions**.  

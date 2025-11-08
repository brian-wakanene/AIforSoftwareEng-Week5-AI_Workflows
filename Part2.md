
## **Part 2: Case Study – Hospital Readmission Risk Prediction**  

### **Problem Scope (5 points)**

**Problem Definition:**  
 *Predict the probability of a patient being readmitted within 30 days of discharge to enable early intervention and reduce costs.*

**Objectives:**  
1. Reduce 30-day readmission rate by 50%.  
2. Prioritize high-risk patients for post-discharge follow-up.  
3. Optimize hospital resource allocation (nurses, case managers).  

**Stakeholders:**  
1. **Hospital Administrators** – reduce penalties under CMS HRRP.  
2. **Clinical Care Teams** – improve patient outcomes and care plans.  



### **Data Strategy**

#### **Proposed Data Sources**  
1. **Electronic Health Records (EHRs)** – diagnosis codes, medications, lab results, length of stay, discharge summary.  
2. **Social Determinants** – age, insurance type, health records.  
3. **Historical admission Logs** 

#### **2 Ethical Concerns**  
**Patient Privacy (HIPAA)** Confidentiality, Risk of re-identification from rare diagnoses. |
**Manual Health records** It is difficult to deal with manual reords eg handwritten discharge summary.

**Preprocessing Pipeline**  

1. Handle Missing Values:
   → Inpute lab results with median per diagnosis group

2. Encode Categorical Variables:
   → One-hot: insurance_type, discharge_disposition

3. Feature Engineering:
   → `num_comorbidities` = count of chronic conditions (e.g., diabetes, CHF)
   → `readmission_history` = binary flag if readmitted in past year
   → `social_risk_score` = PCA on zip-code-level income, education, transport

4. Normalize/Scale:
   → StandardScaler on age, LOS, lab values

**Model Development**

**Selected Model: XGBoost Classifier**  
**Justification:**  
- Handles mixed data types (numerical + categorical).  
- Robust to missing values.  
- Outperforms logistic regression on tabular healthcare data.

**Hypothetical Confusion Matrix** *(Test Set: 1,000 patients)*

|                    | Predicted: No | Predicted: Yes |
|--------------------|---------------|----------------|
| **Actual: No**     | 780 (TN)      | 40 (FP)        |
| **Actual: Yes**    | 60 (FN)       | 120 (TP)       |

- **Precision** = TP / (TP + FP) = 120 / (120 + 40) = **0.75** (75%)  
  → *Of patients flagged high-risk, 75% actually return.*  
- **Recall** = TP / (TP + FN) = 120 / (120 + 60) = **0.67** (67%)  
  → *Model catches 67% of true readmissions.*  

  **F1-Score** = 2 × (0.75 × 0.67) / (0.75 + 0.67) ≈ **0.71**

---

**Deployment**

 **Integration Steps into Hospital System**  

    A[Patient Discharged] --> B[Trigger API Call]
    B --> C[Fetch EHR + Demographics]
    C --> D[Run Preprocessing Pipeline]
    D --> E[XGBoost Model Inference]
    E --> F[Output Risk Score + SHAP]
    F --> G[Push Alert to EMR (Epic/Cerner)]
    G --> H[Notify Case Manager via Dashboard]


 **HIPAA Compliance**  

**Data Encryption**  TLS in transit; AES-256 at rest
**Access Control**  Role-based access (RBAC)
**De-identification** Remove 18 HIPAA identifiers; use limited datasets
**BAA**  Sign Business Associate Agreement with cloud provider



**Optimization (5 points)**

 **Method to Address Overfitting: Early Stopping + Cross-Validation**
Early stopping - This can be achieved by active monitoring validation loss during training and halt when it stops improving and thus  preventing excessive complexity.
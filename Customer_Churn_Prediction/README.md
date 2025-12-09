# 01 - High-Value Customer Churn Prediction (Telecom Case Study)

## 🎯 Project Goal

The primary objective of this project was to develop a predictive model to identify high-risk, **high-value prepaid customers** prone to churning. The analysis focuses on deriving actionable insights to inform a proactive retention strategy, minimizing significant revenue leakage for the telecom firm.

---

## 📈 Methodology & Data Preparation

This project followed a strict business-focused pipeline based on a four-month customer lifecycle (June, July, August, September).

### 1. High-Value Customer Definition
* **Focus Group:** The analysis was restricted to **High-Value Customers (HVCs)**, defined as those whose average recharge amount in the 'good phase' (Months 6 & 7) was **above the 70th percentile**. This filtered the data down to **27,991 HVCs**.
* **Churn Definition:** A customer was tagged as a churner if they had **zero** Minutes of Usage (MOU) for incoming and outgoing calls and zero mobile internet usage in the final month (Month 9).

### 2. Feature Engineering (The Action Phase)
The most critical step was engineering features that capture changes during the 'action phase' (Month 8). Binary features were created to flag significant drops in customer activity, serving as early warning signs for churn:
* `decrease_mou_action`: Significant drop in Minutes of Usage.
* `decrease_rech_num_action`: Drop in the number of recharges.
* `decrease_rech_amt_action`: Drop in the total recharge amount.

---

## 📊 Modeling & Performance

The modeling strategy pursued a dual objective: a **highly accurate prediction model** and an **interpretable model** for business insight.

### 1. Predictive Model (Decision Tree/Random Forest)
* **Goal:** Maximize **Sensitivity/Recall** to ensure maximum identification of actual churners.
* **Performance:** The model achieved a **\[Insert Final Sensitivity Score]% Sensitivity** on the test set.

### 2. Interpretation Model (Logistic Regression)
* **Goal:** Derive **stable coefficients** to pinpoint the exact drivers of churn.
* **Result:** After aggressive feature selection and multicollinearity treatment (VIF reduction), the model successfully identified the primary indicators of churn.

---

## 💡 Key Business Recommendations

Based on the interpretation model's coefficients, the following are the top indicators of churn risk and recommended actions:

1.  **Recharge Consistency is Key:**
    * **Finding:** A **drop in the number of recharges** (`decrease_rech_num_action`) was identified as the single strongest leading indicator of churn.
    * **Recommendation:** Implement an automated, proactive campaign targeting HVCs who miss a single recharge cycle in Month 8. Offer a small, personalized bonus pack to restore their regular usage habits.

2.  **Usage Drop Precedes Churn:**
    * **Finding:** A significant decline in **incoming local call MOU** (`loc_ic_mou_8`) suggests the customer is shifting their primary network, making this a high-risk variable.
    * **Recommendation:** Provide value-added service trials (e.g., unlimited calling to a favorite number) to customers showing a 20%+ drop in incoming calls, locking them into the network for another cycle.

3.  **Data Usage is the Tipping Point:**
    * **Finding:** Changes in monthly 3G/2G volume were significant indicators, confirming the importance of data services for retention.
    * **Recommendation:** For at-risk customers, offer a one-time, high-speed 4G data trial to prevent migration to a competitor based on network speed.

---

## ⚠️ Technical Notes and Future Work

The final model development was time-constrained. The following issues were identified and will be addressed in future versions to increase the robustness and reliability of the interpretation model:

* **Multicollinearity Instability:** Despite RFE, the Logistic Regression model suffered from high VIF (up to $\text{2.7 million}$), leading to unstable coefficients. This requires further iterative VIF-based feature elimination to generate truly robust business insights.
* **Outlier Treatment Flaw:** An error in the initial outlier filtering loop meant that only the last filtering step was correctly applied, incorrectly retaining some outliers. This will be corrected to ensure the dataset is fully cleaned.
* **SMOTE Instability:** The SMOTE oversampling step failed due to highly sparse data points. A more robust technique (e.g., ADASYN or manually adjusting SMOTE parameters) or dropping low-variance features is required.

---

## 💾 Data Source

The dataset used for this analysis is externally hosted and is not included in the repository due to file size constraints.

**Data Download Link:** [Telecom Churn Data](https://drive.google.com/file/d/1SWnADIda31mVFevFcfkGtcgBHTKKI94J/view)

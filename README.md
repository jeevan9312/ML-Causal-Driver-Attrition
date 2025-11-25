# ML-Causal-Driver-Attrition
## Project: Driver Attrition & Causal Inference for a Ride-Sharing Platform

This project applies advanced Exploratory Data Analysis (EDA) and Machine Learning (ML) techniques to a large-scale ride-booking dataset to identify and quantify the primary causes of driver-initiated cancellations, which is a key predictor of driver attrition.

### Business Problem & Objectives

| Category | Description |
| :--- | :--- |
| **Problem** | High driver turnover is costly. The goal is to move beyond simple correlation to identify **causal factors** leading to driver cancellations, enabling management to implement targeted, high-impact policy changes to improve driver retention. |
| **Data Source** | Real-time ride-booking data (`ncr_ride_bookings.csv`) including operational metrics, driver/customer ratings, and booking outcomes. |
| **Target Variable** | **`IS_DRIVER_CANCEL`**: A binary flag indicating whether the booking was cancelled by the driver (The Attrition Signal). |
| **Success Metrics** | **High F1-Score** (predictive accuracy) on the True/Cancellation class, and clear **statistical validation** of the top causal features via Feature Importance. |

---

### Key Findings & Causal Hypotheses (Validated by ML)

Initial EDA and subsequent validation via the Random Forest Classifier confirmed the following three main drivers of operational friction, which are the root causes of cancellation:

| Rank | Causal Factor | Feature Importance | Business Implication & Hypothesis |
| :--- | :--- | :--- | :--- |
| **1** | **Earnings Transparency** | **$\mathbf{34.3\%}$** (Booking\_Value) | **Hypothesis:** Uncertainty about the final payout is the largest source of frustration. Drivers prioritize rides with clear, high profitability. |
| **2** | **Payment Risk/Ambiguity** | **$\mathbf{22.2\%}$** (Payment\_Method\_Unknown) | **Hypothesis:** Unreliable or unknown payment methods (e.g., potential cash fraud, unreliable system) increase the driver's financial risk, leading to proactive cancellation. |
| **3** | **Operational Inefficiency** | **$\mathbf{9.2\%}$** (Avg\_CTAT) | **Hypothesis:** Excessive time spent waiting for the customer (Customer Acceptance Time) is a significant source of wasted time and reduced hourly earnings. |

---
### Power BI: Visualizing the Business Impact

The interactive dashboard translates complex ML findings into actionable business KPIs, helping operational managers monitor risk in real-time.

### Dashboard Key Insights:
* **Cancellation Hotspots:** The **"Sum of Cancelled Rides by Driver by Pickup Location"** chart highlights specific areas (e.g., Nehru Place, Shivaji Park) that are systemic operational failures and need localized support or incentive changes.
* **Vehicle Performance:** The **"Sum of Customer Rating by Vehicle Type"** shows vehicles like **Auto** and **Go Mini** have the highest rating volume, indicating higher customer satisfaction/usage in those segments.
* **Operational Friction:** KPIs show the average **Avg CTAT** (Customer Trip Acceptance Time) and **Avg VTAT** (Vehicle Trip Acceptance Time), allowing for monitoring of overall system efficiency.

<img width="1030" height="588" alt="image" src="https://github.com/user-attachments/assets/b73222ec-3ad1-4fe5-abda-708498db039a" />

### Technical Approach

| Phase | Techniques Used | Key Tool/Library |
| :--- | :--- | :--- |
| **Data Engineering** | Feature creation (`Wait_Time_Ratio`), Robust data cleaning (handling missing location/rating data), and Time-series conversion. | Pandas, NumPy |
| **EDA & Visualization** | Visualization of cancellation rate across binned features (Ratings, Wait Times) to generate causal hypotheses. | Matplotlib, **Seaborn** |
| **Modeling & Validation**| **Random Forest Classifier** (due to speed and interpretability). Used **`class_weight='balanced'`** to handle the high class imbalance between successful and cancelled rides. | Scikit-learn |
| **Causal Validation** | **Random Forest Feature Importance** (Gini Impurity) was used to quantify the statistical influence of each feature on the final cancellation decision. | Scikit-learn |

---

### Machine Learning model:
--- Model Evaluation (Driver Cancellation Prediction) ---
Accuracy Score: 0.9648

Classification Report:
              precision    recall  f1-score   support

       False       1.00      0.96      0.98     23421
        True       0.85      1.00      0.92      5627

    accuracy                           0.96     29048
   macro avg       0.92      0.98      0.95     29048
weighted avg       0.97      0.96      0.97     29048


Confusion Matrix:
[[22399  1022]
 [    0  5627]]

### Business Recommendations (Actionable Interventions)

The model's results (especially the $\mathbf{100\%}$ Recall on the True class) provide high confidence in the following policy changes:

1.  **Enhance Earnings Transparency:** Display the **guaranteed estimated final payout** to the driver *before* they click "Accept" to reduce uncertainty tied to `Booking_Value`.
2.  **Mandate Digital Payment:** Implement policies to **eliminate the "Unknown" payment method** and heavily incentivise digital payments to mitigate the driver's perceived financial risk.
3.  **Customer Readiness Timer:** Introduce a system that charges the customer a fee for excessive waiting (`Avg_CTAT`) and allows the driver a penalty-free cancellation after a short window (e.g., 5 minutes).

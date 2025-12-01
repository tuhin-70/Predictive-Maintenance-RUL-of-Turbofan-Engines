# Predictive Maintenance: Remaining Useful Life (RUL) for Turbofan Engines

## 1. Overview
This project develops a **Predictive Maintenance** solution using machine learning to predict the **Remaining Useful Life (RUL)** of turbofan engines. By accurately predicting the number of cycles an engine can safely run before failure, we enable maintenance teams to shift from expensive, fixed-schedule maintenance to **Condition-Based Monitoring**. This significantly reduces operational costs and prevents catastrophic unplanned failures.

---

## 2. Data Source and Challenge
We utilized the industry-standard **C-MAPSS dataset (FD001)**, which simulates the degradation of 100 distinct turbofan engines until failure. The data includes:
* **Index Features:** Engine ID and Cycle Number (time).
* **Operational Settings:** 3 features (opSetting1, opSetting2, opSetting3).
* **Sensor Readings:** 21 different sensor measurements (sensor1 through sensor21).

The key challenge involved **Feature Engineering** and **Feature Selection** to isolate the few sensor signals that actually degrade over time from the many that remain constant.

---

## 3. Data Analysis and Feature Engineering
### **3.1 Feature Selection**
Based on statistical analysis and visual inspection (plotting Sensor vs. RUL trends):
* We dropped **3 operational settings** which were near-constant in the FD001 subset.
* We dropped **7 sensors** (e.g., sensor1, sensor5, sensor10, sensor18) that showed no degradation trend (constant values) over the engine's lifetime.
* The final model was trained using **14 useful sensors** (2, 3, 4, 7, 8, 9, 11, 12, 13, 14, 15, 17, 20, 21).

### **3.2 RUL Target Calculation**
The **RUL** (our regression target variable) was calculated for every row in the training set:
$$\text{RUL} = \text{Max Life Time Cycle of Engine} - \text{Current Time Cycle}$$

---

## 4. Model Performance
We evaluated several regression models, and the Support Vector Regressor (SVR), trained on the clipped RUL target, yielded the most robust results on the unseen test set.

| Metric | Value | Interpretation |
| :--- | :--- | :--- |
| **Test R² Score** | $\approx 76.7\%$ | The model explains about 76.7% of the variance in the true RUL. |
| **RMSE** | $\approx 20 \text{ cycles}$ | The predicted RUL is, on average, off by about 20 flight cycles. |
| **Model Used** | SVR | Selected for its strong generalization performance. |

---

## 5. Real-World Business Cost Framework
The model delivers a clear return on investment (ROI) by applying a standard cost framework to the predicted RUL.

| Financial Metric | Assumption / Value | Calculation |
| :--- | :--- | :--- |
| **Baseline Unplanned Cost** (per engine/year) | $2\% \times \$150,000$ | **\$3,000** |
| Savings from avoided failures (40% reduction) | $0.40 \times \$3,000$ | **+\$1,200** |
| Extra planned maintenance cost | $0.18 \times \$5,000$ | **-\$900** |
| **Net Saving per Engine per Year** | $\$1,200 - \$900$ | **\$300** |

This analysis confirms the model's viability by demonstrating a tangible **\$300 net saving** for every engine in the fleet each year.

---

## 6. Repository Contents
* `Predictive_Maintenance_RUL_of_Turbofan_Engines.ipynb`: Complete Jupyter notebook with EDA, Feature Engineering, Model Training, and Financial Analysis.
* `train_FD001.txt`, etc.: Datasets used (external source).
* `README.md`: This documentation file.
---

## 7. Tech Stack
* **Programming Language:** Python 3
* **Libraries:**
    * Data Manipulation: Pandas, NumPy
    * Visualization: Matplotlib, Seaborn
    * Machine Learning: Scikit-learn (LinearRegression, SVR)
    * Metrics: `r2_score`, `mean_squared_error`

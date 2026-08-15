# Customer Churn Prediction Using Machine Learning

## Project Overview

Customer churn can reduce revenue, profitability, and long-term customer value. Identifying customers at risk of leaving allows businesses to prioritize retention efforts before disengagement becomes permanent.

This project develops an end-to-end machine learning framework to predict customer churn using historical transaction data from a UK-based online retailer. A time-based prediction approach is used so that customer behavior observed before a defined cutoff date is used to predict future inactivity.

The analysis goes beyond traditional RFM metrics by engineering additional behavioral features related to customer engagement, purchasing patterns, transaction value, and product diversity.

---

## Business Problem

The project addresses three primary business questions:

- Which customers are most likely to become inactive?
- Which historical purchasing behaviors are most strongly associated with future churn?
- How can churn predictions be translated into actionable customer retention strategies?

The objective is not simply to build the most accurate classification model, but to develop a framework that can help businesses identify at-risk customers and prioritize retention efforts.

---

## Dataset

The project uses the **Online Retail dataset from the UCI Machine Learning Repository**, containing transactional data from a UK-based online retailer.

The original dataset contains more than **540,000 transactions** recorded between December 2010 and December 2011.

Transaction-level variables include:

- Customer ID
- Invoice number
- Invoice date
- Product description
- Quantity purchased
- Unit price
- Country

The transaction data is transformed into customer-level behavioral features for churn analysis and predictive modeling.

---

## Methodology

### 1. Data Cleaning and Preparation

The transactional data is prepared by addressing missing customer identifiers, invalid transactions, cancellations, date formatting, and other data-quality issues required for reliable customer-level analysis.

### 2. Time-Based Churn Framework

A temporal framework is used to reduce target leakage.

Transactions before the prediction cutoff date are used to construct customer behavioral features, while purchasing activity during a subsequent observation window is used to determine whether each customer remained active or churned.

This approach more closely reflects a real-world churn prediction setting because the model only receives information that would have been available at the time the prediction was made.

### 3. Feature Engineering

Customer-level features include traditional RFM measures and additional behavioral indicators.

**RFM Features**
- Recency
- Frequency
- Monetary Value

**Purchasing Behavior**
- Average Basket Value
- Average Quantity per Invoice

**Shopping Diversity**
- Unique Invoices
- Unique Products
- Product Diversity

**Customer Engagement**
- Customer Tenure
- Active Months
- Average Purchase Interval
- Purchase Regularity

**Geographic Profile**
- Country
- UK Customer Indicator

A selected subset of these features is used for predictive modeling.

### 4. Exploratory Data Analysis

Exploratory analysis examines:

- Customer geographic composition
- Customer spending concentration
- Purchase frequency
- Average basket value
- Customer engagement patterns
- Relationships between behavioral features and future churn
- Differences in recency, frequency, and monetary value between active and churned customers

### 5. Predictive Modeling

Four classification approaches are evaluated:

- Logistic Regression
- Decision Tree
- Random Forest
- Tuned Random Forest

Models are compared using multiple evaluation metrics rather than accuracy alone.

---

## Model Performance

| Model | Accuracy | ROC-AUC | Churn Precision | Churn Recall | Churn F1-Score |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.651 | 0.710 | 0.575 | 0.579 | 0.577 |
| Decision Tree | 0.654 | 0.703 | 0.586 | 0.538 | 0.561 |
| Random Forest | 0.625 | 0.693 | 0.548 | 0.502 | 0.524 |
| **Tuned Random Forest** | **0.655** | **0.721** | **0.576** | **0.612** | **0.593** |

The **Tuned Random Forest** achieved the strongest overall balance across the evaluation metrics and was selected as the final model.

Its churn recall of **61.2%** indicates that the model identified approximately six out of every ten customers who subsequently became inactive in the test set.

---

## Key Findings

### Customer inactivity is an important churn signal

Customers who later churned generally had higher recency values, indicating that they had already gone longer without purchasing before the prediction cutoff date.

### Purchase frequency provides meaningful retention information

Customers who purchased more frequently were generally more likely to remain active, while lower purchasing frequency was associated with greater future churn risk.

### Historical spending alone does not guarantee retention

Although active customers generally exhibited higher historical spending, substantial overlap existed between active and churned customers. Some high-value customers still became inactive, showing that customer value and customer retention should not be treated as the same concept.

### Engagement features add useful behavioral information

The tuned Random Forest identified **Active Months** as the most important feature, followed by **Frequency**, **Monetary Value**, and **Recency**.

This suggests that sustained engagement over time may provide particularly useful information when identifying customers at risk of churn.

---

## Estimated Business Impact

The final model identified **290 customers** in the test set as being at risk of future churn.

These customers collectively represented approximately **£90,081.23** in historical spending.

A scenario-based retention simulation illustrates the potential customer value associated with successful interventions:

| Retention Success Scenario | Customers Retained | Historical Spending Represented |
|---|---:|---:|
| 10% | 29 | £9,008 |
| 20% | 58 | £18,016 |
| 30% | 87 | £27,024 |

These figures are illustrative rather than realized financial outcomes. They demonstrate how churn predictions can be translated into a business framework for prioritizing retention efforts and estimating the potential value associated with successful interventions.

---

## Business Recommendations

Based on the analysis, businesses could:

- Monitor recency and declining purchase frequency as early indicators of customer disengagement.
- Use churn probabilities to prioritize customers for targeted retention campaigns.
- Pay particular attention to previously valuable customers whose engagement has recently declined.
- Incorporate broader engagement measures, such as active purchasing months, rather than relying exclusively on total historical spending.
- Test retention interventions and measure their actual incremental impact through controlled experiments.

---

## Limitations

Several limitations should be considered when interpreting the results:

- Churn is defined using purchasing inactivity during a specified future observation period rather than explicit account cancellation.
- Historical transaction data does not capture factors such as customer satisfaction, marketing exposure, website activity, or competitor behavior.
- The business-impact simulation uses historical spending as a proxy for customer value and does not account for campaign costs or future customer purchasing behavior.
- Model performance may change when applied to different retailers, customer populations, or time periods.

---

## Future Improvements

Future versions of the project could explore:

- Additional behavioral and temporal features
- Alternative churn definitions and prediction windows
- Gradient boosting models such as XGBoost or LightGBM
- Probability-threshold optimization based on retention costs and expected customer value
- Customer lifetime value integration
- Explainability techniques such as SHAP
- Controlled retention experiments to measure actual campaign effectiveness

---

## Tools and Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook

---

## Repository Contents

- `customer_churn_prediction.ipynb` – Complete analysis, feature engineering, exploratory analysis, modeling, evaluation, and business-impact simulation
- `README.md` – Project overview and key findings

---

## How to Run the Project

1. Download the Online Retail dataset from the **UCI Machine Learning Repository**.
2. Place the dataset in the same working directory as the notebook.
3. Ensure the dataset filename matches the filename referenced in the notebook.
4. Install the required Python libraries.
5. Open `customer_churn_prediction.ipynb` in Jupyter Notebook, JupyterLab, or Google Colab.
6. Run the notebook cells sequentially.

---

## Conclusion

This project demonstrates how transactional data can be transformed into a practical customer churn prediction framework.

The results suggest that churn risk is influenced not only by how much customers spend, but also by **how recently, how frequently, and how consistently they engage with the business**.

By combining behavioral feature engineering, time-based validation, machine learning, and business-impact simulation, the project illustrates how predictive analytics can support more proactive and data-driven customer retention strategies.

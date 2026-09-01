# Customer Churn Prediction Using Machine Learning

## Project Overview

Customer churn can reduce revenue, profitability, and long-term customer value. Identifying customers at risk of becoming inactive can provide businesses with an early-warning signal for customer retention planning.

This project develops an end-to-end machine learning framework to predict customer churn using historical transaction data from a UK-based online retailer. A time-based prediction framework is used so that customer behavior observed before a defined cutoff date is used to predict future purchasing inactivity.

The analysis goes beyond traditional RFM metrics by engineering additional behavioral features related to customer engagement, purchasing patterns, transaction value, and product diversity.

Importantly, the project distinguishes between **predicting churn risk** and **deciding which customers should receive a retention intervention**. Churn probability can identify customers who may be at risk, but effective intervention targeting would also require information about customer value, treatment responsiveness, and intervention cost.

---

## Business Problem

The project addresses three primary business questions:

- Which customers are most likely to become inactive?
- Which historical purchasing behaviors provide useful information for predicting future churn?
- How can churn predictions contribute to a broader framework for customer retention decision-making?

The objective is not simply to build the most accurate classification model, but to develop a practical framework for identifying at-risk customers while recognizing the additional evidence required to make effective retention decisions.

---

## Dataset

The project uses the **Online Retail dataset from the UCI Machine Learning Repository**, containing transactional data from a UK-based online retailer.

The original dataset contains **541,909 transactions** recorded between December 2010 and December 2011.

Transaction-level variables include:

- Customer ID
- Invoice number
- Invoice date
- Product description
- Quantity purchased
- Unit price
- Country

After data cleaning, the analysis contains **392,692 valid transactions representing 4,338 customers**.

The transaction data is subsequently transformed into customer-level behavioral features for churn analysis and predictive modeling.

---

## Methodology

### 1. Data Cleaning and Preparation

The transactional data is prepared by addressing missing customer identifiers, invalid transactions, cancellations, date formatting, and other data-quality issues required for reliable customer-level analysis.

### 2. Time-Based Churn Framework

A temporal framework is used to separate the predictor period from the outcome period.

Transactions before **September 1, 2011** are used to construct customer behavioral features, while purchasing activity between **September 1 and December 9, 2011** is used to determine whether each historical customer remained active or became inactive.

For customers observed during the historical period:

- **Active (0):** Made at least one purchase during the future observation period.
- **Churned (1):** Made no purchase during the future observation period.

Churn therefore represents **observed inactivity during the available future window rather than confirmed permanent customer loss**.

This temporal separation reduces the risk of target leakage because the model predictors are constructed only from information that would have been available at the prediction date.

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

A selected subset of these features is used for predictive modeling:

- Recency
- Frequency
- Monetary Value
- Customer Tenure
- Active Months
- Average Basket Value
- Product Diversity

### 4. Exploratory Data Analysis

Exploratory analysis examines:

- Customer geographic composition
- Historical customer spending concentration
- Purchase frequency
- Average basket value
- Customer engagement patterns
- Relationships between behavioral features and future churn
- Differences in recency, frequency, monetary value, tenure, active months, basket value, and product diversity between active and churned customers

### 5. Predictive Modeling

Four classification approaches are evaluated:

- Logistic Regression
- Decision Tree
- Random Forest
- Tuned Random Forest

The customer-level modeling dataset is divided into stratified training and held-out testing sets. Models are compared using multiple evaluation metrics rather than accuracy alone, with particular attention to ROC-AUC and performance on the churn class.

---

## Model Performance

| Model | Accuracy | ROC-AUC | Churn Precision | Churn Recall | Churn F1-Score |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.651 | 0.710 | 0.575 | 0.579 | 0.577 |
| Decision Tree | 0.654 | 0.703 | 0.586 | 0.538 | 0.561 |
| Random Forest | 0.625 | 0.693 | 0.548 | 0.502 | 0.524 |
| **Tuned Random Forest** | **0.655** | **0.721** | **0.576** | **0.612** | **0.593** |

The **Tuned Random Forest** achieved the strongest overall performance among the models evaluated and was selected as the final predictive model.

On the held-out test set, it achieved:

- **Accuracy:** 65.5%
- **ROC-AUC:** 0.721
- **Churn Recall:** 61.2%
- **Churn F1-Score:** 0.593

Its churn recall indicates that the model identified approximately six out of every ten customers who subsequently became inactive.

The improvements over Logistic Regression were modest, but the Tuned Random Forest provided the best overall combination of discrimination and churn-class identification among the models tested.

---

## Key Findings

### Customer inactivity is an important churn signal

Customers who later churned generally had higher recency values, indicating that they had already gone longer without purchasing before the prediction cutoff date.

Recency therefore provides a useful early-warning signal of future purchasing inactivity.

### Purchase frequency provides meaningful predictive information

Customers who purchased more frequently were generally more likely to remain active, while lower purchasing frequency was associated with greater future churn risk.

### Sustained engagement is particularly informative

The Tuned Random Forest identified **Active Months** and **Frequency** as the strongest contributors to its churn predictions.

**Monetary Value** and **Recency** also contributed meaningfully, while Average Basket Value, Customer Tenure, and Product Diversity made smaller contributions within the fitted model.

These feature-importance results represent **predictive contribution rather than causal influence**. They do not establish that changing these behaviors would itself reduce churn.

### Historical spending alone does not determine retention

Although historical spending contains useful information about customer behavior and economic value, customer value and churn risk are distinct concepts.

A customer can have substantial historical spending and still become inactive. Likewise, high churn risk does not establish whether a customer would respond to a retention intervention.

---

## Business Impact Scenario Analysis

The final model identified **290 customers** in the held-out test set as likely to churn.

These customers collectively represented approximately **£90,081.23 in historical spending**, with average historical spending of approximately **£310.62 per predicted churner**.

To illustrate the scale of historical customer value represented by this group, three hypothetical retention scenarios were examined:

| Hypothetical Retention Rate | Customers Retained Under Scenario | Historical Spending Represented |
|---:|---:|---:|
| 10% | 29 | £9,008.12 |
| 20% | 58 | £18,016.25 |
| 30% | 87 | £27,024.37 |

These figures are **descriptive scenario illustrations rather than forecasts, realized savings, or causal estimates of a retention campaign's financial impact**.

Historical spending is used as an initial proxy for customer value and should not be interpreted as expected future revenue or Customer Lifetime Value. The assumed retention rates are hypothetical and are not estimated from intervention data.

The analysis therefore demonstrates how churn predictions and customer value information can contribute to retention planning without assuming that high-risk customers are automatically the customers most likely to benefit from intervention.

---

## From Prediction to Retention Decision-Making

A useful retention strategy requires more than identifying customers with high churn probabilities.

Four considerations can contribute jointly to a retention allocation decision:

1. **Churn risk** — Who is likely to become inactive?
2. **Customer value** — What economic value is associated with retaining the customer?
3. **Treatment responsiveness** — Whose behavior is likely to change because of the intervention?
4. **Intervention cost** — Is targeting the customer economically worthwhile?

The current project estimates the first component and uses historical spending as an initial proxy related to the second. It does **not** estimate treatment responsiveness or causal intervention effects.

A stronger future targeting framework could evaluate expected incremental economic value, conceptually combining:

**Incremental probability of retention × expected future customer value − intervention cost**

Estimating the incremental effect of a retention intervention would require appropriate causal evidence, ideally from randomized treatment and control data. Such data could subsequently support uplift or heterogeneous treatment-effect modeling.

---

## Business Recommendations

Based on the analysis, businesses could:

- Use churn probabilities as an **early-warning signal** for potential customer inactivity.
- Monitor recency, declining purchase frequency, and sustained engagement patterns as indicators of changing customer behavior.
- Combine predicted churn risk with measures of expected customer value when assessing the economic importance of at-risk customers.
- Avoid treating churn probability alone as an automatic retention-targeting rule.
- Test retention interventions through controlled experiments to determine whether they actually change customer behavior.
- Incorporate intervention costs and incremental treatment effects when moving from churn prediction to retention resource allocation.

---

## Limitations

Several limitations should be considered when interpreting the results:

- Churn is defined using purchasing inactivity during a specified future observation period rather than explicit account cancellation.
- Historical transaction data does not capture factors such as customer satisfaction, marketing exposure, website activity, or competitor behavior.
- Historical spending is used as a proxy related to customer value and does not represent expected future revenue or Customer Lifetime Value.
- The business-impact scenarios use hypothetical retention rates and should not be interpreted as causal estimates or realized financial outcomes.
- The available data does not identify which customers would respond to a retention intervention.
- Intervention costs are not included in the analysis.
- Feature importance represents predictive contribution rather than causal influence.
- Model performance may change when applied to different retailers, customer populations, or time periods.

---

## Future Improvements

Future versions of the project could explore:

- Additional behavioral and temporal features
- Alternative churn definitions and prediction windows
- Gradient boosting models such as XGBoost or LightGBM
- Probability-threshold optimization based on business costs and expected customer value
- Expected future customer value or Customer Lifetime Value modeling
- Explainability techniques such as SHAP
- Controlled retention experiments to measure causal campaign effectiveness
- Uplift or heterogeneous treatment-effect modeling using appropriate intervention data
- Decision frameworks that jointly consider churn risk, customer value, treatment responsiveness, and intervention cost

---

## Tools and Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook / Google Colab

---

## Repository Contents

- `customer_churn_prediction.ipynb` – Complete analysis, feature engineering, exploratory analysis, predictive modeling, model evaluation, and business-impact scenario analysis
- `README.md` – Project overview, methodology, key findings, business interpretation, limitations, and future improvements

---

## How to Run the Project

1. Download the Online Retail dataset from the **UCI Machine Learning Repository**.
2. Place the dataset in the same working directory as the notebook.
3. Ensure the dataset filename matches the filename referenced in the notebook.
4. Install the required Python libraries.
5. Open `customer_churn_prediction.ipynb` in Jupyter Notebook, JupyterLab, or Google Colab.
6. Restart the runtime/kernel and run the notebook cells sequentially from top to bottom.

---

## Conclusion

This project demonstrates how transactional data can be transformed into a practical customer churn prediction framework.

The results show that historical purchasing behavior contains meaningful information about future customer inactivity. In particular, the timing, frequency, and consistency of customer engagement provide useful predictive signals.

The project also demonstrates an important distinction between **prediction and decision-making**. A churn model can estimate who is at risk, while effective retention allocation requires additional consideration of customer value, treatment responsiveness, and intervention cost.

By combining behavioral feature engineering, temporal separation of predictors and outcomes, machine learning, held-out evaluation, and business scenario analysis, the project provides a foundation for data-driven customer retention decision-making while remaining explicit about what observational transaction data can and cannot establish.

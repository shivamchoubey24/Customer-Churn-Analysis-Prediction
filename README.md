# Customer Churn Analysis & Prediction

An end-to-end telecom customer churn project that combines a **machine learning prediction model (Python/scikit-learn)** with an **interactive Power BI dashboard**, going beyond descriptive analytics to actively flag which currently-active customers are most likely to churn next.

## 📌 Business Problem

Customer churn directly erodes recurring revenue for subscription-based telecom businesses. Rather than just reporting *how much* churn happened last period, this project aims to answer two questions:
1. **Diagnostic:** Which customer segments, contracts, services, and regions have the highest churn rates, and why?
2. **Predictive:** Among customers currently active, which ones are most at risk of churning next — so retention teams can act *before* they leave?

## 🗂️ Dataset

- **Source:** [`Customer_Data.csv`](./Customer_Data.csv) / [`Prediction_Data.xlsx`](./Prediction_Data.xlsx)
- **Size:** 6,418 customer records, 30 fields
- **Two views used for modeling:**
  - `vw_ChurnData` — historical customers labeled **Stayed** or **Churned** (used to train the model)
  - `vw_JoinData` — newly joined/currently active customers with unknown future status (the model scores these)
- **Key fields:** Demographics (Gender, Age, Married, State), account details (Tenure, Contract, Payment Method, Value Deal), services subscribed (Internet Type, Streaming, Security, Backup, Premium Support), and financials (Monthly Charge, Total Charges, Total Revenue, Total Refunds)

## 🛠️ Tools & Workflow

| Phase | Tools | What was done |
|---|---|---|
| **1. Data Modeling** | Power Query / Excel | Structured churn data into `vw_ChurnData` (labeled historical data) and `vw_JoinData` (new customers to score) |
| **2. Predictive Modeling (Python)** | Pandas, scikit-learn, Seaborn | Label-encoded categorical features, trained a **Random Forest Classifier**, evaluated performance, and scored new customers — see [`Churn_Prediction.ipynb`](./Churn_Prediction.ipynb) |
| **3. Visualization (Power BI)** | Power BI Desktop | Built a 3-page dashboard: Summary, Churn Reason, and Churn Prediction, with the model's output fed back in as a table of at-risk customers |

### Model Details
- **Algorithm:** Random Forest Classifier (100 trees)
- **Train/test split:** 80/20
- **Accuracy:** ~86%
- **Churn class (positive) performance:** 83% precision, 66% recall, 0.74 F1-score
- **Top predictive features:** Total Revenue, Contract type, Total Charges, Monthly Charge, Total Long Distance Charges, and Age

Model predictions for new/active customers are exported to [`Predictions.csv`](./Predictions.csv) and surfaced directly in the dashboard's **Churn Prediction** page.

## 📊 Key Insights (Summary Page)

- **6,418 total customers**, **1,732 churned (27.0% churn rate)**, with **411 new joiners**
- **Month-to-Month contracts churn the most by far (46.5%)**, vs. just 11.0% for One Year and 2.7% for Two Year contracts — contract length is the single biggest lever against churn
- Churn skews toward customers **over 35** (the 35–50 and 50+ age bands show the highest churn rates on the age-group chart)
- **Fiber Optic** internet users churn at the highest rate among internet types (41.1%), notably higher than Cable (25.7%) or DSL (19.4%)
- **Jammu & Kashmir (57.2%) and Assam (38.1%)** are the top two states by churn rate — a small number of states drive disproportionate churn
- **Competitor offers are the #1 churn category (761 customers)**, ahead of attitude-related issues (301) and dissatisfaction (300)
- Mailed Check payment users churn far more (37.8%) than Credit Card users (14.8%)

## 🔮 Prediction Results (Churn Prediction Page)

- The model flags **365 currently active customers as predicted churners**
- Skews female (235) vs. male (130), and toward the **35–50 (132)** and **20–35 (125)** age groups
- **Month-to-Month contracts account for 355 of the 365** predicted churners — reinforcing the contract-type finding from the descriptive analysis
- Credit Card (184) and Bank Withdrawal (147) are the most common payment methods among at-risk customers
- A full customer-level table (ID, monthly charge, revenue, refunds, referrals) lets retention teams act on named accounts, not just aggregate stats

## 🖥️ Dashboard Pages

1. **Summary** — company-wide churn KPIs, demographic/account/geographic/service breakdowns
2. **Churn Reason** — churn category and reason breakdown
3. **Churn Prediction** — model output: predicted churner profile and a filterable, customer-level "at risk" table

## 🚀 How to Reproduce

1. Open [`Churn_Prediction.ipynb`](./Churn_Prediction.ipynb) and point it at [`Prediction_Data.xlsx`](./Prediction_Data.xlsx) to retrain the model and regenerate [`Predictions.csv`](./Predictions.csv)
2. Open [`Churn_Dashboard.pbix`](./Churn_Dashboard.pbix) in [Power BI Desktop](https://powerbi.microsoft.com/desktop/)
3. Refresh the data source to pull in updated predictions, then explore via the Monthly Charge, Married, and other slicers

## 📁 Repository Structure

```
├── Churn_Dashboard.pbix           # Power BI dashboard file
├── Customer_Data.csv              # Historical customer dataset
├── Prediction_Data.xlsx           # Structured views (vw_ChurnData, vw_JoinData)
├── Predictions.csv                # Model output: predicted churners
├── Churn_Prediction.ipynb         # Python ML notebook (Random Forest)
├── summary.png
├── prediction.png
└── README.md
```

## 🔮 Possible Next Steps

- Improve recall on the churn class (currently 66%) — try class weighting, SMOTE, or threshold tuning, since missing an actual churner is costlier than a false alarm
- Add a monthly-refresh pipeline so predictions update automatically as new customer data lands
- Layer in a customer lifetime value (CLV) estimate to prioritize retention efforts by revenue impact, not just churn probability

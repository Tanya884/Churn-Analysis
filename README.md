# 📉 Customer Churn Analysis — SQL + Power BI

End-to-end churn analysis of a telecom customer base (India, 6,418 customers): data cleaning and feature engineering in **SQL Server**, followed by an interactive **Power BI** dashboard that shows who is leaving, where, and why.

## 🎯 Business Problem
Which customers are leaving, why are they leaving, and which segments should the business focus on to reduce churn?

## 🛠️ Tools Used
- **SQL Server (T-SQL)**: data profiling, cleaning, imputation, feature engineering, views
- **Power BI Desktop**: dashboard and DAX measures
- **Dataset**: 6,418 customers × 32 columns (demographics, services, contract, billing, churn category/reason)

## 📁 Repository Structure
```
Churn_Analysis/
├── Data.csv                          # Raw dataset
├── CHURN_PREDICATION_ANALYSIS.SQL    # Cleaning + analysis queries
├── Churn_Analysis.pbix               # Power BI dashboard
├── Churn_Analysis.pdf                # Dashboard export
└── README.md
```

## 🔧 SQL Workflow
1. **Data profiling:** duplicate `Customer_ID` check, state-wise counts, NULL audit.
2. **Missing-value imputation:** `Value_Deal` and `Internet_Type` filled with the **most common value per state** (CTE + `ROW_NUMBER()` window function + `UPDATE ... JOIN`).
3. **Feature engineering:**
   - `AGE_BUCKET`: Young (≤25), Mid-Young (26–35), Mid-Old (36–45), Old (46+)
   - `REVENUE_CATEGORY`: Low (≤49), Medium (50–149), Premium (150+)
4. **Exploratory queries:** gender split, contract mix, status vs. revenue contribution, state distribution.
5. **Production table:** `PROD_CHURN` with NULL add-on services set to `0` and missing churn category/reason set to `'OTHERS'`.
6. **Views:** `VW_CHURN_DATA` (Churned + Stayed, for modelling) and `VW_JOIN_DATA` (newly joined customers).

## 📊 Key Insights
| Finding | Number |
|---|---|
| Customers: Stayed / Churned / Joined | 4,275 / 1,732 / 411 |
| Overall churn rate (all customers) | **27%** (28.8% excluding new joiners) |
| Share of total revenue lost to churned customers | **17.5%** |
| Month-to-Month contract churn | **52.4%** vs 11.2% (1-yr) and 2.8% (2-yr) |
| Fiber Optic churn | **42.5%** vs 20.8% (DSL) |
| Mailed Check payers churn | **42.9%** vs 16.2% (Credit Card) |
| Customers **without** Online Security / Premium Support | ~45% churn vs ~15% with them |
| Top churn category | **Competitor: 761 customers (~44% of churners)** |
| Highest-churn states (excl. new joiners) | Jammu & Kashmir (58.5%), Assam (41.1%), Jharkhand (37.1%) |

## 💡 Recommendations
- Offer incentives to move **month-to-month** customers onto 1- or 2-year plans.
- Investigate **Fiber Optic** service quality and pricing, since it has the highest churn.
- Bundle **Online Security and Premium Support** as retention add-ons.
- Counter competitor offers with better device and data deals, since competitors account for ~44% of churn.
- Improve support-agent training: "attitude" and "dissatisfaction" are the next largest churn categories.
- Run targeted retention campaigns in high-churn states.

## ▶️ How to Reproduce
1. Import `Data.csv` into SQL Server as a table named `DATA` in a database named `PROJECT`.
2. Run `CHURN_PREDICATION_ANALYSIS.SQL` step by step.
3. Open `Churn_Analysis.pbix` in Power BI Desktop (connect it to `PROD_CHURN` / the views if you want a refresh).

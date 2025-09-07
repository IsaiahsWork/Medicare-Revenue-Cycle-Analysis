# 🏥 Medicare Revenue Cycle Analysis (2008–2010)

The healthcare revenue cycle is critical for financial sustainability—ensuring timely reimbursements, minimizing bad debt, and optimizing operational cash flow. This SQL-based project analyzes Medicare billing claims data between 2008–2010 to uncover trends, detect payment delays, and forecast future revenue behavior. 

📂 Data Source  
This project uses the DE-SynPUF Medicare inpatient claims dataset (2008–2010) provided by CMS.  
Full dataset available in `Data Source` file.  

More details: [Data Source link](https://www.cms.gov/data-research/statistics-trends-and-reports/medicare-claims-synthetic-public-use-files/cms-2008-2010-data-entrepreneurs-synthetic-public-use-file-de-synpuf)

---

## 📦 Dataset Overview

The DE-SynPUF inpatient claims dataset simulates realistic Medicare claim data while preserving HIPAA compliance. It spans 2008–2010 and contains key fields:

| Column Name                   | Description                  |
| ----------------------------- | ---------------------------- |
| CLM\_ID                       | Claim Identifier             |
| CLM\_THRU\_DT                 | Claim End Date               |
| CLM\_PMT\_AMT                 | Claim Payment Amount         |
| CLM\_DRG\_CD                  | Diagnosis Related Group Code |
| NCH\_PRMRY\_PYR\_CLM\_PD\_AMT | Primary Payer Contribution   |
| CLM\_ADMSN\_DT                | Admission Date               |
| ICD9\_DGNS\_CD\_x             | Diagnosis Codes              |
| HCPCS\_CD\_x                  | Procedure Codes              |

---

## 🔍 Analysis Objectives

✔️ **Total Revenue Collected** – How much was paid overall
✔️ **Unpaid Claims (Denials)** – Identifying volume of non-reimbursed claims
✔️ **Primary Payer Contributions** – Detecting insurer/Medicare involvement
✔️ **Average Payment Per Claim** – Forecasting and cost benchmarking
✔️ **Monthly Revenue Trends** – Seasonality & operational planning
✔️ **Forecasting** – SQL-based cash flow prediction

---

## 🧪 Revenue and Claims Breakdown

### ✅ Query Used:

```sql
SELECT TOP(10)
  CLM_DRG_CD,
  COUNT(CLM_ID) AS Total_Claims,
  SUM(CLM_PMT_AMT) AS Total_Paid_Amount,
  AVG(CLM_PMT_AMT) AS Avg_Paid_Amount,
  SUM(CASE WHEN CLM_PMT_AMT = 0 THEN 1 ELSE 0 END) AS Unpaid_Claims_Count,
  SUM(CASE WHEN CLM_PMT_AMT > 0 THEN CLM_PMT_AMT ELSE 0 END) AS Total_Collected,
  SUM(CASE WHEN CLM_PMT_AMT = 0 THEN NCH_PRMRY_PYR_CLM_PD_AMT ELSE 0 END) AS Primary_Payer_Contributions
FROM [SqlProjects].[dbo].[DE1_0_2008_to_2010_Inpatient_Claims_Sample_2]
GROUP BY CLM_DRG_CD
ORDER BY Total_Paid_Amount DESC;
```

📷 *Refer to Image 2 (Revenue and Claims Query)*

### 📊 Results:

📷 *Refer to Image 3 (Claims Results)*

* Most top-paid claims were fully reimbursed by Medicare.
* Minimal contribution required from primary payers.
* Indicates efficient reimbursement processing in high-cost DRG codes.

---

## 📅 Monthly Revenue Trends

### ✅ Query Used:

```sql
SELECT
  YEAR(CLM_THRU_DT) AS Year,
  MONTH(CLM_THRU_DT) AS Month,
  SUM(CLM_PMT_AMT) AS Total_Collected
FROM [SqlProjects].[dbo].[DE1_0_2008_to_2010_Inpatient_Claims_Sample_2]
WHERE CLM_PMT_AMT > 0 AND CLM_THRU_DT IS NOT NULL
GROUP BY YEAR(CLM_THRU_DT), MONTH(CLM_THRU_DT)
ORDER BY Year, Month;
```

📷 *Refer to Image 4 (Monthly Revenue Trends Query)*

### 📊 Results:

📷 *Refer to Image 5 (Monthly Revenue Trends Results)*

* Revenue fluctuated between **\$20M and \$24M per month**.
* **December consistently spiked**, likely due to flu season and end-of-year procedures.
* **May 2009** saw the **highest monthly revenue at \$23.1M**.

---

## 📈 Forecasting Future Revenue

### ✅ Query Used:

```sql
WITH Revenue_Trends AS (
  SELECT
    YEAR(CLM_THRU_DT) AS Year,
    MONTH(CLM_THRU_DT) AS Month,
    SUM(CLM_PMT_AMT) AS Total_Collected,
    LAG(SUM(CLM_PMT_AMT)) OVER (ORDER BY YEAR(CLM_THRU_DT), MONTH(CLM_THRU_DT)) AS Prev_Month_Revenue
  FROM [SqlProjects].[dbo].[DE1_0_2008_to_2010_Inpatient_Claims_Sample_2]
  WHERE CLM_PMT_AMT > 0 AND CLM_THRU_DT IS NOT NULL
  GROUP BY YEAR(CLM_THRU_DT), MONTH(CLM_THRU_DT)
)
SELECT
  Year,
  Month,
  Total_Collected,
  Prev_Month_Revenue,
  (Total_Collected - Prev_Month_Revenue) AS Month_Over_Month_Change,
  ROUND(AVG(Total_Collected) OVER (ORDER BY Year, Month ROWS BETWEEN 5 PRECEDING AND CURRENT ROW), 2) AS Moving_Avg_6_Months,
  ROUND((Total_Collected + (Total_Collected - Prev_Month_Revenue)), 2) AS Next_Month_Forecast
FROM Revenue_Trends
ORDER BY Year, Month;
```

📷 *Refer to Image 6 (Forecast Query)*

### 📊 Results:

📷 *Refer to Image 7 (Forecast Results)*

* Revenue in **2009 was stable** (\$18M–\$23M/month).
* A **decline in 2010** suggests potential operational or economic issues.
* SQL forecasting highlighted downturns early, helping with cash flow planning.

---

## 🔑 Key Takeaways

* 💸 **Delayed Payments Affect Cash Flow** – Some claims show \$0 reimbursement
* 🏥 **Primary Payers Drive Revenue** – Medicare accounts for most payments
* 📈 **Revenue is Seasonal** – Higher in December due to flu/elective surgery surges
* 🔮 **Forecasting Enables Planning** – SQL-based models support financial foresight

---

## 📢 Final Thoughts

SQL-powered analysis of the Medicare revenue cycle can uncover inefficiencies, support better planning, and aid in denial management. This project demonstrates how structured claims data can be transformed into actionable insights for financial leadership in healthcare.

**Let’s connect** – [LinkedIn](https://linkedin.com) | [Portfolio](https://yourportfolio.com)

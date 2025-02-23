# Medicare-Revenue-Cycle-Analysis

The healthcare revenue cycle is critical for financial sustainability, ensuring timely reimbursements and reducing bad debt. By analyzing hospital billing data, we can identify payment delays, improve collections, and optimize revenue flow.

📊 What I Analyzed
For this analysis, I focused on key financial metrics within the hospital billing system:

✔️ Total Revenue Collected – Understanding actual hospital revenue. 

✔️ Unpaid Claims (Possible Denials) – Identifying gaps in payments. 

✔️ Primary Payer Contributions – Evaluating the role of insurers and Medicare. 

✔️ Average Payment Per Claim – Essential for forecasting and financial planning. 

✔️ Monthly Revenue Trends – Detecting seasonality and cash flow fluctuations.

Let's take a look at the data:

The dataset was retrieved from here and covers two years (2008-2010) of billing data from Medicare and Medicaid services. The DE-SynPUF dataset was designed to provide realistic claims data while safeguarding Medicare beneficiaries' protected health information. Key variables include Claims Date, Admission Date, Claim Payment Amount, Provider Institution, Diagnosis Code, among others.


Table
To evaluate revenue collection and claim statuses, I used SQL techniques like CASE WHEN to classify claims, the SUM aggregation to find the total claim amount for each category and then used the COUNT aggregation to evaluate the amount of Total Claims.




Revenue and Claims Query



Revenue and Claims Results
Using ORDER BY on the Diagnosis Related Group Code (DRG Code), I identified which illnesses resulted in the highest total paid amounts and how many claims remained unpaid. If the Total Collected amount matches the Total Paid Amount, this suggests full reimbursement. Most claims appear to be fully collected without additional contributions from primary payers, indicating efficient payment processing for those claims.

📅 Monthly Revenue Trends
Next, I analyzed the Monthly Revenue Trends. Understanding monthly revenue trends helps identify seasonal patterns and predict cash flow. I filtered date data using the CLM_THRU_DT column (end of claim), by YEAR and MONTH time functions and NOT NULL to exclude all missing dates.


Monthly Revenue Trends Query



Monthly Revenue Trends Results
Findings from Monthly Revenue Trends
Revenue fluctuates within a narrow range (~$20M to $24M per month).
December ($23M) has one of the highest revenues, likely due to: Seasonal trends in healthcare (e.g., flu season, elective surgeries) impact revenue collection patterns.

To predict cash flow trends, I created a Common Table Expression (CTE) using the LAG function to retrieve the previous month’s revenue for comparison.


Forecast Query



Forecast Query
Findings from Revenue Forecasting
2009 showed stable revenue (between $18M - $23M).
The highest revenue month was May 2009 ($23.1M).
2010 saw a steep decline, indicating potential financial concerns.

🔑 Key Takeaways
✔️ Delayed Payments Affect Cash Flow – A significant portion of claims remain unpaid, indicating possible denials or processing issues. 

✔️ Primary Payers Drive Revenue – Medicare, Medicaid, and insurers contribute the bulk of payments, underscoring their impact on financial health. 

✔️ Revenue is Seasonal – Payment trends fluctuate, requiring proactive financial planning. 

✔️ Forecasting Improves Planning – SQL analytics help predict cash flow and optimize hospital financial management.

📢 Final Thoughts
Analyzing healthcare billing data with SQL provides valuable insights into revenue performance, payment trends, and areas for improvement. As I continue refining my analytical skills, I welcome feedback and discussions—feel free to connect!

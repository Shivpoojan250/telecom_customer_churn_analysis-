# telecom_customer_churn_analysis-
End-to-end Telecom Customer Churn Analysis project using SQL, Python (Pandas), and Power BI. Includes data cleaning, exploratory analysis, churn trends, revenue impact, customer lifetime value, and interactive dashboards. Identified 1,869 churned customers (~26%), $3.7M revenue at risk, and key retention opportunities.


SQL

----find the overall churned customer 

select* from telco_churn_cleaned;


select count(*) as total_customer , SUM(CASE WHEN Customer_Status= 'Churned'then 1 else 0 end) as churned_customer,
ROUND( SUM(CASE WHEN Customer_Status= 'Churned'then 1 else 0 end)/count(*) *100,2)
from telco_churn_cleaned;


----- 3. CHURN RATE BY TENURE BUCKET
select Gender, count(*) as total_customer , SUM(CASE WHEN Customer_Status='Churned' then 1 else 0 end) as total_churned,
ROUND(SUM(CASE WHEN Customer_status='Churned' then 1 else 0 end)/COUNT(*)*100,2)
from telco_churn_cleaned
group by Gender;


----- 5. CHURN RATE BY Churn category 

select Churn_Category , count(*) as total_customers , SUM(CASE WHEN CUstomer_Status='Churned' then 1 else 0 end) as total_churned_customer,
ROUND(SUM(CASE WHEN Customer_Status= 'Churned' then 1 else 0 end)/count(*) *100,2) as churned_customer_by_Category 
from telco_churn_cleaned
Group by Churn_Category;



----- 8. TOP CHURN REASONS (only churned customers)

select Top 10 Churn_Reason , SUM(CASE WHEN Customer_Status = 'Churned' then 1 else 0 end ) as total_churned
from telco_churn_cleaned
Group by Churn_Reason
Order by total_churned DESC;

-- 7. AVERAGE CLTV: CHURNED vs ACTIVE

select Customer_Status, AVG(CLTV) AS avg_cltv 
from telco_churn_cleaned
group by Customer_Status;



---- 10. SATISFACTION SCORE vs CHURN

select Satisfaction_score,  SUM(CASE when Customer_Status = 'Churned'then 1 else 0 end) as total_churned
from telco_churn_cleaned
group by  Satisfaction_score
Order by 
Satisfaction_score, total_churned desc;


-- 11. RUNNING TOTAL OF CHURNED CUSTOMERS BY TENURE
-- (Window function - cumulative churn as tenure increases)


select  Contract , sum(CASE when Customer_Status= 'Churned' then 1 else 0 end),
SUM(SUM( CASE When Customer_Status ='Churned' then 1 else 0 end))
over(Order by Contract DESC)
from telco_churn_cleaned
Group by  Contract 
;



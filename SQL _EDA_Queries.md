Task 1: Data Download, Import, and Database Connection
# -- Load the sql extention ----
%load_ext sql

# --- Load your mysql db using credentials from the "DB" area ---
%sql mysql+pymysql://<user>:<password>@localhost/<db_name>

Task 2: Average Hospital Charges Analysis
In this project, we aim to analyze a medical dataset to determine the average hospital charges. This analysis can provide essential insights into healthcare cost trends, helping hospitals and patients understand the financial aspects of medical care. By calculating the average hospital charges, we gain valuable information for financial planning, cost optimization, and transparency in healthcare services.

SELECT AVG(charges) FROM hospitalization_details 
----------------------------------------------------------------------------------------------------------------------------
Task 3: High Charges Analysis
This project focuses on identifying unique customer identifiers, corresponding years, and charges from a specific medical dataset, specifically for records where charges exceed 700. By retrieving this data, we can gain insights into cases of exceptionally high hospital charges, which can inform further investigation, cost control strategies, and patient financial support.

SELECT customer_id,year,charges
FROM hospitalization_details
where charges>700
----------------------------------------------------------------------------------------------------------------------------
Task 4: High BMI Patients Analysis
In this project, we aim to retrieve the name, year, and charges for customers with a BMI (Body Mass Index) greater than 35 from a medical dataset. Analyzing the data of high BMI patients allows us to understand the healthcare costs associated with this specific group. This information can be valuable for identifying health trends, managing patient care, and optimizing medical expenses.

SELECT n.name,hd.year,hd.charges
FROM medical_examinations me 
join names n on me.customer_id=n.customer_id
join hospitalization_details hd
on me.customer_id=hd.customer_id
where BMI > 35
----------------------------------------------------------------------------------------------------------------------------
Task 5: Customers with Major Surgeries
This project focuses on listing customer IDs and names of individuals from the names table who have undergone major surgeries, as recorded in the medical_examinations table. By identifying such patients, we can gain insights into the population with a history of major surgical procedures, which can inform healthcare planning, risk assessment, and medical follow-up.

SELECT me.customer_id,name
from names n JOIN medical_examinations me
on n.customer_id=me.customer_id
where numberofmajorsurgeries >=1
----------------------------------------------------------------------------------------------------------------------------
Task 6: Average Charges by Hospital Tier in 2000
In this project, we aim to calculate the average hospital charges per hospital tier for the year 2000 from the hospitalization_details table. This analysis allows us to understand the variation in charges based on the hospital tier, providing insights into cost disparities and healthcare quality across different tiers. It can assist in making informed decisions about healthcare facilities and costs.

SELECT hospital_tier, AVG(charges)
from hospitalization_details
where year=2000
GROUP by hospital_tier
----------------------------------------------------------------------------------------------------------------------------
Task 7: Smoking Patients with Transplants Analysis
This project aims to retrieve customer IDs, BMI, and charges for patients who are smokers and have undergone a transplant, as per the medical_examinations and hospitalization_details tables. Analyzing this data allows us to study the healthcare costs and health conditions of patients with a history of smoking and transplants. This information can be valuable for targeted healthcare interventions and cost estimation.

SELECT hd.customer_id, BMI, charges
FROM hospitalization_details hd 
JOIN medical_examinations me 
on hd.customer_id=me.customer_id
WHERE smoker="yes" and any_transplant="yes"
----------------------------------------------------------------------------------------------------------------------------
Task 8: Patients with Major Surgeries or Cancer History
In this project, we retrieve the names of customers who have had at least two major surgeries or have a history of cancer, as recorded in the medical_examinations table. This analysis helps identify patients with complex medical histories, enabling healthcare providers to tailor care plans and assess potential healthcare costs for these individuals.

SELECT name
FROM medical_examinations me 
join names n 
on n.customer_id=me.customer_id
WHERE numberofmajorsurgeries>=2 or cancer_history="yes" 
----------------------------------------------------------------------------------------------------------------------------
Task 9: Customer with Most Major Surgeries¶
In this project, we identify and display the customer with the highest number of major surgeries. By joining the names and medical_examinations tables and sorting the records by the number of major surgeries in descending order, we can pinpoint the customer with the most significant surgical history. This insight is valuable for personalized healthcare management and resource allocation.

SELECT name,me.customer_id
FROM medical_examinations me 
join names n 
on n.customer_id=me.customer_id
ORDER by numberofmajorsurgeries DESC
LIMIT 1
----------------------------------------------------------------------------------------------------------------------------
Task 10: Customers with Major Surgeries and City Tiers
In this project, we compile a list of customers who have undergone major surgeries and their respective cities' tier levels (city_tier) from the hospitalization_details table. This analysis provides insights into the distribution of major surgeries across different city tiers, aiding in healthcare planning, resource allocation, and assessing the impact of city tiers on surgical cases.

SELECT name,hd.customer_id,city_tier
FROM medical_examinations me 
join names n on me.customer_id=n.customer_id
join hospitalization_details hd
on me.customer_id=hd.customer_id
WHERE numberofmajorsurgeries>0
----------------------------------------------------------------------------------------------------------------------------
Task 11: Average BMI by City Tier in 1995
This project aims to calculate the average BMI for each city tier level in the year 1995 from the hospitalization_details table. Analyzing the average BMI across different city tiers allows us to understand the variations in health parameters among urban areas. It provides insights that can be used for health planning, resource allocation, and identifying potential health trends.

SELECT city_tier, AVG(BMI)
FROM medical_examinations me 
join hospitalization_details hd
on me.customer_id=hd.customer_id
where year="1995"
GROUP by city_tier
----------------------------------------------------------------------------------------------------------------------------
Task 12: High BMI Customers with Health Issues
In this project, we extract customer IDs, names, and charges of customers who have health issues and a BMI greater than 30. By combining data from the names, medical_examinations, and hospitalization_details tables, we can identify individuals with specific health concerns and high BMI levels. This information is valuable for targeted healthcare interventions and assessing associated healthcare costs.

SELECT hd.customer_id,name,charges
FROM medical_examinations me 
join names n on me.customer_id=n.customer_id
join hospitalization_details hd
on me.customer_id=hd.customer_id
where BMI>30 and health_issues='yes'
----------------------------------------------------------------------------------------------------------------------------
Task 13: Customers with Highest Charges and City Tier by Year
In this project, we identify the customer with the highest total charges for each year and display their corresponding city_tier. By joining the hospitalization_details and names tables and grouping the data by year, customer name, and city_tier, we can determine which customer incurred the highest charges in each year. This analysis is crucial for understanding cost patterns over time and tailoring healthcare strategies accordingly.

SELECT year,name, hd.city_tier,max(hd.charges)
FROM hospitalization_details hd
join names n 
on n.customer_id=hd.customer_id
GROUP by year,name, hd.city_tier
having max(hd.charges) = (SELECT max(charges) FROM hospitalization_details where year=hd.year)
---------------------------------------------------------------------------------
Task 14: Top 3 Customers with Highest Average Yearly Charges
This project focuses on identifying the top 3 customers with the highest average yearly charges over the years they have been hospitalized. By calculating and analyzing the average yearly charges from the hospitalization_details data and joining it with customer names, we can pinpoint those individuals with the highest healthcare expenditure. Understanding these patterns is essential for resource allocation and tailored healthcare planning.
%%sql

WITH YearlyCharges AS (
  SELECT h.customer_id, h.year, AVG(h.charges) AS avg_yearly_charge
  FROM hospitalization_details h
  GROUP BY h.customer_id, h.year
)
SELECT n.name, MAX(yc.avg_yearly_charge) AS highest_avg_yearly_charge
FROM YearlyCharges yc
JOIN names n ON yc.customer_id = n.Customer_ID
GROUP BY n.name
ORDER BY highest_avg_yearly_charge DESC
LIMIT 3;

or 

WITH YearlyCharges AS (
  SELECT 
    h.customer_id, 
    h.year, 
    AVG(h.charges) AS avg_yearly_charge,
    RANK() OVER (PARTITION BY h.year ORDER BY AVG(h.charges) DESC) AS yearly_rank
  FROM hospitalization_details h
  GROUP BY h.customer_id, h.year
)
SELECT n.name, yc.avg_yearly_charge AS highest_avg_yearly_charge
FROM YearlyCharges yc
JOIN names n ON yc.customer_id = n.Customer_ID
WHERE yc.yearly_rank = 1 -- Selecting the highest average yearly charge for each year
ORDER BY yc.avg_yearly_charge DESC
LIMIT 3;
-----------------------------------------------------------------------------------------
Task 15: Ranking Customers by Total Charges
This analysis aims to rank customers based on their total charges over the years in descending order. By summing up the charges from the hospitalization_details data for each customer and assigning a rank, we can identify those with the highest healthcare expenses. This information is valuable for healthcare providers and policymakers in tailoring services and managing resources effectively.

%%sql
SELECT n.name, SUM(h.charges) AS total_charges,
       RANK() OVER (ORDER BY SUM(h.charges) DESC) AS charges_rank
FROM hospitalization_details h
JOIN names n ON h.customer_id = n.Customer_ID
GROUP BY n.name
ORDER BY charges_rank;

OR 

select name, sum(charges) as total_chrg,
rank() over(ORDER by sum(charges) DESC ) as rnk
from hospitalization_details hd
join names n 
on hd.customer_id=n.customer_id
GROUP by name
order by rnk
-------------------------------------------------------------------------------------------------------------------------
Task 16: Identifying Peak Year for Hospitalizations
This task is essential for identifying the year with the highest number of hospitalizations. By calculating the count of hospitalizations for each year from the hospitalization_details dataset, we can pinpoint the peak year for healthcare demand. This insight can help healthcare institutions allocate resources and plan for peak demand years more effectively.

%%sql
with cte AS (
SELECT year, count(customer_id) as YearlyHospitalizations,
    	rank() over(ORDER by count(customer_id) desc) as rnk
FROM hospitalization_details
GROUP by year
    )
select year, YearlyHospitalizations
from cte
where rnk=1

or

%%sql
WITH YearlyHospitalizations AS (
  SELECT year, COUNT(*) AS num_hospitalizations
  FROM hospitalization_details
  GROUP BY year
)
SELECT year, num_hospitalizations
FROM YearlyHospitalizations
WHERE num_hospitalizations = (SELECT MAX(num_hospitalizations) FROM YearlyHospitalizations);
------------------------------------------------------------------------------------------------------------------------








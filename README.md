# LITA-CUSTOMER-DATA-PROJECT

[PROJECT OVERVIEW](#project-overview)

[DATA SOURCE](#data-source)

[DATA TOOLS](#data-tools)

[DATA PREPARATION](#data-preparation)

[EXPLORATORY DATA ANALYSIS](#exploratory-data-analysis)

[DATA ANALYSIS](#data-analysis)

## DATA VISUALIZATION

## FINDINGS AND RESULT

## RECOMMENDATION

### project overview
  The goal of this project is to analyze customer subscription data to extract valuable insights that can drive decision-making, enhance customer retention, and maximize revenue. The analysis will focus on customer demographics, subscription types, regional trends, and financial performance.

  ### data source
   This Dataset will be given on the repository. This entails Sales data from a retail company (which include transaction details, product categories, customer demographics, and regional data).

   ### data tools
  . Excel for Data cleaning, transformation, and initial exploratory analysis.
  
  . SQL for Database creation, data manipulation, and complex querying for deeper insights.
  
  . Power BI for Data visualization and dashboard creation to present findings.

  ### data preparation
 Data Cleaning and Preparation in Excel
 
 •	Load the Data: Import the customer data file into Excel.
 
 •	Remove Duplicates: Eliminate any duplicate customer records.
 
 •	Convert Revenue: Strip commas from the Revenue column and convert it to a numeric data type for calculations.
 
 •	Create new calculated fields, like total sales, subscription Duration, average subscription duration

 ### exploratory data analysis
 -	Analyze customer data using pivot tables to find subscription patterns.
 -	Calculate the average subscription duration
 -	What are the most popular subscription types.
 -	What is total number of customers from each region. 
 -	  customers who canceled their subscription within 6 months.
 -	What is the average subscription duration for all customers.
 -	  customers with subscriptions longer than 12 months.
 -	 What is the total revenue by subscription type.
 -	 The top 3 regions by subscription cancellations.
 -	 What is the total number of active and canceled subscriptions.

   ### data analysis
#### TABLE 1(FINDINGS AND RESULT) 

![customer table 1](https://github.com/user-attachments/assets/0d35d7bc-07ba-4a59-a935-b046830d5771)

  This table entails Revenue by subscription type, there are basically 3 types of subscription which are BASIC, PREMIUM AND STANDARD Basic has the highest Revenue with a value of 33,776,737 while Standard has the lowest Revenue with a value of 16,864,376

#### RECOMDATION

Base on this Analysis, the company should have a promo package for STANDARD AND PREMIUM SUBSCRIPTION


#### TABLE 2(FINDINGS AND RESULT)


![customer table 2](https://github.com/user-attachments/assets/29228e3a-edcb-4b53-90da-2414c8cf4b63)

This Table entails Revenue by customers cancelation, the Sum of 37,208,727 were made from order that was not canceled while 30,331,448 were made from orders that was canceled.

#### RECOMDATION

There should be a promo package for customers without canceled order 

#### TABLE 3(FINDINGS AND RESULT)

![customer table3](https://github.com/user-attachments/assets/316d741a-d873-48c4-b97a-a22ba4d563f3)


This Table entails Revenue by Region and subscription type, according to our findings BASIC subscription is only found in EAST Region with the sum of 16,958,367 and in NORTH region with the sum of 16,817,972.
PREMIUM subscription is only found in SOUTH region with Total Revenue of 16,899,064 while STANDARD subscription is only found in WEST region with total Revenue of 16,864,376.

#### RECOMDATION
There should be a promo on STANDARD and PREMIUM so other Region can buy it.

#### TABLE 4(FINDINGS AND RESULT)

![customer table 4](https://github.com/user-attachments/assets/4a7a3d48-d0ba-4fda-8f3d-a129cd5d5942)

This table entails Top ten customers which are LIAM, MIKE, SOPHIA, NINA, ANNA, JANE, GRACE, EVA, JOHN, ALEX.

#### RECOMDATION

There should be a special promo or award for top subscribers 

#### SQL ANALYSIS

-	The file was converted to csv format and imported into SQL 
-	Customer data Table were created 

QUERY 1
```sql
select * from [dbo].[capstonecustomerdata]
```
(NUMBER OF CUSTOMER PER REGION)
SELECT COUNT(customerName) AS totalcustomer, region
FROM [dbo].[capstonecustomerdata]
WHERE region IN ('north', 'south', 'west', 'east')
GROUP BY region
ORDER BY region
QUERY 2 (MOST POPULAR SUBSCRIPTION TYPE BY NUMBER OF CUSTOMER)
SELECT 
    subscriptionType,
    COUNT(customerName) AS CustomerCount
FROM 
    [dbo].[capstonecustomerdata]
GROUP BY 
    subscriptionType
ORDER BY 
    CustomerCount DESC
QUERY 2 (CUSTOMER THAT CANCELED THEIR SUBSCRIPTION WITHIN 6MONTHS)
SELECT customerName
FROM [dbo].[capstonecustomerdata]
WHERE canceled = 'true' 
AND SubscriptionEnd >= DATEADD(MONTH, -6, GETDATE()) 
ORDER BY SubscriptionEnd DESC
QUERY 3 (SUBSCRIPTION DURATION FOR EACH CUSTOMER)
SELECT 
    customerName, 
    SUM(DATEDIFF(DAY, 
                 CONVERT(datetime, subscriptionStart, 120), 
                 CONVERT(datetime, subscriptionEnd, 120))) AS subscriptionDuration
From
    [dbo].[capstonecustomerdata]
GROUP BY 
    customerName
ORDER BY 
    SubscriptionDuration
QUERY 4(AVERAGE SUBSCRIPTION DURATION)
SELECT 
    AVG(subscriptionDuration) AS averageSubscriptionDuration
FROM (
    SELECT 
        DATEDIFF(DAY, subscriptionStart, subscriptionEnd) AS subscriptionDuration
    FROM 
        [dbo].[capstonecustomerdata]
    WHERE
        subscriptionStart IS NOT NULL AND
        subscriptionEnd IS NOT NULL
) AS customerDurations
QUERY 5 (CUSTOMERS SUBSCRIPTION THAT IS MORE THAN 12 MONTHS)
SELECT
    customerName,
    DATEDIFF(MONTH, 
             CONVERT(datetime, subscriptionStart, 120), 
             CONVERT(datetime, subscriptionEnd, 120)) AS subscriptionDurationInMonths
FROM 
    [dbo].[capstonecustomerdata]
WHERE 
    subscriptionStart IS NOT NULL AND
    subscriptionEnd IS NOT NULL
    AND DATEDIFF(MONTH, 
                 CONVERT(datetime, subscriptionStart, 120), 
                 CONVERT(datetime, subscriptionEnd, 120)) > 12
QUERY 6 (TOTAL REVENUE BY SUBSCRIPTION TYPE)
select
subscriptiontype,
sum(revenue) as totalrevenue
from [dbo].[capstonecustomerdata]
group by SubscriptionType
order by totalrevenue
QUERY 13(TOP 3 REGION BY SUBSCRIPTION CANCELATION)
SELECT TOP 3 region, COUNT(*) AS cancellation_count
FROM [dbo].[capstonecustomerdata]
WHERE canceled = 'TRUE'
GROUP BY region
ORDER BY cancellation_count DESC
QUERY 14(TOTAL NUMBER OF ACTIVE AND CANCELED SUBSCRIPTION)
SELECT 
  SUM(CASE WHEN canceled ='TRUE' THEN 1 ELSE 0 END) AS totalcanceled,
  SUM(CASE WHEN canceled = 'FALSE' THEN 1 ELSE 0 END) AS totalactive
FROM [dbo].[capstonecustomerdata]



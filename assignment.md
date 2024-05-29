Follow the instructions below to complete this project.

## Part 1: Loading csv Files into Database

Create a new PostgreSQL database called `ecommerce`. Set up tables for each .csv file by following [these instructions](https://www.postgresqltutorial.com/postgresql-tutorial/import-csv-file-into-posgresql-table/)




--DROP TABLE products

CREATE TABLE products (
	 SKU VARCHAR (20)
	,ProductName VARCHAR (100)
	,orderedquantity INTEGER
	,stockLevel INTEGER
	,restockingLeadTime INTEGER
	,sentimentScore FLOAT
	,sentimentMagnitude FLOAT
	,PRIMARY KEY (SKU)
	

);
	

--DROP TABLE sales_by_sku

CREATE TABLE sales_by_sku (
	 SKU VARCHAR (20)
	,total_ordered INTEGER

);

--DROP TABLE sales_report

CREATE TABLE sales_report (
	 SKU VARCHAR (20)
	,total_ordered INTEGER
	,ProductName VARCHAR (100)
	,stockLevel INTEGER
	,restockingLeadTime INTEGER
	,sentimentScore FLOAT
	,sentimentMagnitude FLOAT
	,Ratio FLOAT
	,PRIMARY KEY (SKU)
	

);

--DROP TABLE all_Sessions

CREATE TABLE all_Sessions (
	 fullVisitorId FLOAT
	,channelGrouping VARCHAR (25)
	,sessiontime INT
	,country VARCHAR (50)
	,city VARCHAR (50)
	,totalTransactionRevenue FLOAT
	,transactions FLOAT
	,timeOnSite INT
	,pageviews INT
	,sessionQualityDim INT
	,sessiondate DATE
	,visitId BIGINT
	,sessiontype VARCHAR (25)
	,productRefundAmount FLOAT
	,productQuantity INT
	,productPrice FLOAT
	,productRevenue FLOAT
	,productSKU VARCHAR (20)
	,v2ProductName VARCHAR (100)
	,v2ProductCategory VARCHAR (50)
	,productVariant VARCHAR (50)
	,currencyCode VARCHAR (3)
	,itemQuantity INT
	,itemRevenue FLOAT
	,transactionRevenue FLOAT
	,transactionId VARCHAR (100)
	,pageTitle VARCHAR (1500)
	,searchKeyword VARCHAR (50)
	,pagePathLevel1 VARCHAR (50)
	,eCommerceAction_type INT
	,eCommerceAction_step INT
	,eCommerceAction_option VARCHAR (100)
	,PRIMARY KEY (visitID,productSKU,sessiontime)
	

);


--create staging table fpr data cleaning
CREATE TABLE ANALYTICS_staging (
	 visitNumber NUMERIC(21, 0)
	,visitId FLOAT
	,visitStartTime FLOAT
	,sessiondate DATE
	,fullvisitorId NUMERIC(21, 0)
	,userid NUMERIC(21, 0)
	,channelGrouping VARCHAR (50)
	,socialEngagementType VARCHAR (50)
	,units_sold INT
	,pageviews INT
	,timeonsite FLOAT
	,bounces INT
	,revenue FLOAT
	,unit_price FLOAT
)


--create actual analytics table you will place cleaned data into
CREATE TABLE ANALYTICS (
	 visitNumber NUMERIC(21, 0)
	,visitId FLOAT
	,visitStartTime FLOAT
	,sessiondate DATE
	,fullvisitorId NUMERIC(21, 0)
	,userid NUMERIC(21, 0)
	,channelGrouping VARCHAR (50)
	,socialEngagementType VARCHAR (50)
	,units_sold INT
	,pageviews INT
	,timeonsite FLOAT
	,bounces INT
	,revenue FLOAT
	,unit_price FLOAT
	,PRIMARY KEY (visitID,unit_price)
)

--clean and push data to the actual analytics table
WITH cleaned_data AS (
    SELECT DISTINCT ON (visitId, unit_price) -- ensuring no duplicate primary keys
        visitNumber, visitId, visitStartTime, sessiondate, fullvisitorId, userid, channelGrouping, socialEngagementType, units_sold, pageviews, timeonsite, bounces, revenue, unit_price
    FROM analytics_staging
    ORDER BY visitId, unit_price
)
INSERT INTO analytics (visitNumber, visitId, visitStartTime, sessiondate, fullvisitorId, userid, channelGrouping, socialEngagementType, units_sold, pageviews, timeonsite, bounces, revenue, unit_price)
SELECT 
    visitNumber, visitId, visitStartTime, sessiondate, fullvisitorId, userid, channelGrouping, socialEngagementType, units_sold, pageviews, timeonsite, bounces, revenue, unit_price
FROM cleaned_data;

-- Step 4: Update unit_price
UPDATE analytics
SET unit_price = unit_price / 1000000;

-- Step 5: delete the staging table
DROP TABLE analytics_staging;




> #### Note
> We have gone over the ways that you can import a .csv file into the PostgreSQL database but [this resource](https://www.youtube.com/watch?v=6Jf7eTkIaR4) summarizes it, in case you need a refresher.


## Part 2: Data Cleaning

As always, once you have received any dataset, your first task is to orient yourself to the data contained within. While exploring the data, you should keep an eye out for any of potential data issues that need to be cleaned. 

**Cleaning hint**: The unit cost in the data needs to be divided by 1,000,000. 

Apart from this, did you see any other issue that requires cleaning? Be sure to take the time upfront to address them.

In your copy of the **cleaning_data.md** file, describe what issues you addressed by cleaning the data and provide the queries you executed to clean the data.



UPDATE all_sessions
SET productprice = productprice / 1000000;

UPDATE all_sessions 
SET totalTransactionRevenue = totalTransactionRevenue / 1000000;

UPDATE all_sessions 
SET productRevenue = Product Revenue / 1000000;

UPDATE all_sessions 
SET transactionRevenue = transactionRevenue / 1000000;

--cleaning data for analytics
--clean and push data to the actual analytics table, so their is a primary key
WITH cleaned_data AS (
    SELECT DISTINCT ON (visitId, unit_price) -- ensuring no duplicate primary keys
        visitNumber, visitId, visitStartTime, sessiondate, fullvisitorId, userid, channelGrouping, socialEngagementType, units_sold, pageviews, timeonsite, bounces, revenue, unit_price
    FROM analytics_staging
    ORDER BY visitId, unit_price
)

UPDATE analytics -- completed
SET unit_price = unit_price / 1000000;

UPDATE analytics -- completed
SET revenue = revenue / 1000000;


ALTER TABLE analytics -- make time column readable
ALTER COLUMN visitstarttime TYPE TIME
USING '00:00:00'::time + (visitstarttime || ' seconds')::interval;

ALTER TABLE analytics -- make time column readable
ALTER COLUMN timeonsite TYPE TIME
USING '00:00:00'::time + (timeonsite || ' seconds')::interval;

UPDATE all_sessions
SET city = 'Unknown'
WHERE city = 'not available in demo dataset';

UPDATE all_sessions
SET city = 'Unknown'
WHERE city = '(not set)';

UPDATE all_sessions
SET country = 'Unknown'
WHERE country = '(not set)';


## Part 3: Starting with Questions

This database provides data on revenue by product as well as data on how each visitor to the site interacted with the products (when they visited, where they were git status
based, how many pages they viewed, how long they stayed on the site, and the number of transactions they entered).
 
In the **starting_with_questions.md** file there are 5 questions you need to answer using the data. For each question, be sure to include:
The queries you used to answer the question
The answer to the question
 

## Part 4: Starting with Data

Consider the data you have available to you.  You can use the data to:
    - find all duplicate records
    - find the total number of unique visitors (`fullVisitorID`)
    - find the total number of unique visitors by referring sites
    - find each unique product viewed by each visitor
    - compute the percentage of visitors to the site that actually makes a purchase
    

In the **starting_with_data.md** file, write 3 - 5 new questions that you could answer with this database. For each question, include
The queries you used to answer the question
The answer to the question
    

## Part 5: QA Your Data

In the **QA.md** file, identify and describe your risk areas. Develop and execute a QA process to address them and validate the accuracy of your results. Provide the SQL queries used to execute the QA process.


## Part 6: Generate the ERDLoading Your Final Table Into PostgreSQL Database

Inside pgAdmin, you can generate the ERD for the database. 

![](https://i.imgur.com/KxVRJD3.png)

Download the image and save it as **schema.png**. Add this file to your repo for submission.

Note: If you have created any new tables, be sure to load them into the database before printing the ERD!

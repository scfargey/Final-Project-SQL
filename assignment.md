Follow the instructions below to complete this project.

## Part 1: Loading csv Files into Database

Create a new PostgreSQL database called `ecommerce`. Set up tables for each .csv file by following [these instructions](https://www.postgresqltutorial.com/postgresql-tutorial/import-csv-file-into-posgresql-table/)




CREATE TABLE products (
	 SKU VARCHAR (20)
	,ProductName VARCHAR (50)
	,orderedquantity INTEGER
	,stockLevel INTEGER
	,restockingLeadTime INTEGER
	,sentimentScore FLOAT
	,sentimentMagnitude FLOAT
	,PRIMARY KEY (SKU)
	

);

DROP TABLE sales_by_sku

CREATE TABLE sales_by_sku (
	 SKU VARCHAR (20)
	,total_ordered INTEGER
	,stockLevel INTEGER
	,restockingLeadTime INTEGER
	,sentimentScore FLOAT
	,sentimentMagnitude FLOAT
	,PRIMARY KEY (SKU)
);



CREATE TABLE sales_report (
	 SKU VARCHAR (20)
	,total_ordered INTEGER
	,ProductName VARCHAR (50)
	,stockLevel INTEGER
	,restockingLeadTime INTEGER
	,sentimentScore FLOAT
	,sentimentMagnitude FLOAT
	,Ratio FLOAT
	,PRIMARY KEY (SKU)
	

);


CREATE TABLE all_Sessions (
	 fullVisitorId BIGINT
	,channelGrouping VARCHAR (25)
	,sessiontime NUMERIC
	,country VARCHAR (50)
	,city VARCHAR (50)
	,totalTransactionRevenue FLOAT
	,transactions FLOAT
	,timeOnSite FLOAT
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
	,v2ProductName VARCHAR (50)
	,v2ProductCategory VARCHAR (50)
	,productVariant VARCHAR (50)
	,currencyCode VARCHAR (3)
	,itemQuantity INT
	,itemRevenue FLOAT
	,transactionRevenue FLOAT
	,transactionId FLOAT
	,pageTitle VARCHAR (50)
	,searchKeyword VARCHAR (50)
	,pagePathLevel1 VARCHAR (50)
	,eCommerceAction_type INT
	,eCommerceAction_step INT
	,eCommerceAction_option INT
	,PRIMARY KEY (visitID)
	

);


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
	,PRIMARY KEY (visitID)
)





> #### Note
> We have gone over the ways that you can import a .csv file into the PostgreSQL database but [this resource](https://www.youtube.com/watch?v=6Jf7eTkIaR4) summarizes it, in case you need a refresher.


## Part 2: Data Cleaning

As always, once you have received any dataset, your first task is to orient yourself to the data contained within. While exploring the data, you should keep an eye out for any of potential data issues that need to be cleaned. 

**Cleaning hint**: The unit cost in the data needs to be divided by 1,000,000. 

Apart from this, did you see any other issue that requires cleaning? Be sure to take the time upfront to address them.

In your copy of the **cleaning_data.md** file, describe what issues you addressed by cleaning the data and provide the queries you executed to clean the data.

## Part 3: Starting with Questions

This database provides data on revenue by product as well as data on how each visitor to the site interacted with the products (when they visited, where they were based, how many pages they viewed, how long they stayed on the site, and the number of transactions they entered).
 
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

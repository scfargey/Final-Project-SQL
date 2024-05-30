What are your risk areas? Identify and describe them.



QA Process:
Describe your QA process and include the SQL queries used to execute it.

My QA process starts by ensuring that each table has the ability to have a primary key, which can be solved by checking for duplicates. 

I would then check to make sure that data is being imprted consitently in the correct format ie, pricing, revenue, etc... that was done in the cleaning portion of this project. 


```SQL  
--ensure no duplicate SKU's are in the products table

SELECT
	p.SKU

FROM 
	products p

GROUP BY p.SKU
	
HAVING COUNT(p.SKU)>1


--ensure no duplicate product names, noted that there are many duplicates, and realistically each product should have a
SELECT
	p.productname
	,COUNT(p.productname)

FROM 
	products p

GROUP BY p.productname
	
HAVING COUNT(p.productname)>1 --noted that there are many duplicates, and realistically each product should have anunique name, especially on an ecommerce platform

--ensure no duplicates in the sales by sku table

SELECT
	p.SKU

FROM 
	sales_by_sku p

GROUP BY p.SKU
	
HAVING COUNT(p.SKU)>1

--ensure no duplicates in the sales_report table

SELECT
	p.SKU

FROM 
	sales_report p

GROUP BY p.SKU
	
HAVING COUNT(p.SKU)>1

--the analytics table has no primary key or combination of columns for a primary key, so at this point is a useless table of data

--the all_sessions table has a mix of three columns for a primary key, us this query to ensure no duplicates
SELECT
    visitId,
    productSKU,
    sessiontime,
    COUNT(*) AS cnt
FROM
    all_Sessions
GROUP BY
    visitId,
    productSKU,
    sessiontime
HAVING
    COUNT(*) > 1;


--if i was to redeisgn this database i would need a better understanding of where this data is coming from and then ensure it is in 3NF by splitting the tables accordingly. 
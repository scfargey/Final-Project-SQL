Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
```sql
--Countries with the Highest Transaction Revenue
SELECT
	 a.country
	,ROUND(SUM(totalTransactionRevenue):: NUMERIC, 2) AS Transaction_Rev

FROM
	all_sessions a

WHERE
	a.Country <> 'Unknown'
GROUP BY
	a.country
	
HAVING
	SUM(a.totaltransactionrevenue) > 0
	
ORDER BY SUM(a.totaltransactionrevenue) DESC
```
```sql
--cities with the highest transaction revenue
SELECT
	 a.city
	,ROUND(SUM(totalTransactionRevenue):: NUMERIC, 2) AS Transaction_Rev

FROM
	all_sessions a

WHERE
	a.City <> 'Unknown'
GROUP BY
	a.City
	
HAVING
	SUM(a.totaltransactionrevenue) > 0
	
ORDER BY SUM(a.totaltransactionrevenue) DESC
```

Answer:
The US has the highest transaction revenue and San Fransisco is the Highest City transaction Revenue



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
```sql
--avg productquantity by country
SELECT
     a.country
    ,ROUND(AVG(a.productQuantity)::NUMERIC,2) AS Avg_Products_Ordered
FROM
    all_sessions a
WHERE
    productQuantity IS NOT NULL
    AND country <> 'Unknown'
GROUP BY
    a.country
ORDER BY
    Avg_Products_Ordered DESC;
```

```sql
--avg productquantity by city
SELECT
     a.city
    ,ROUND(AVG(a.productQuantity)::NUMERIC,2) AS Avg_Products_Ordered
FROM
    all_sessions a
WHERE
    productQuantity IS NOT NULL
    AND a.city <> 'Unknown'
GROUP BY
    a.city
ORDER BY
    Avg_Products_Ordered DESC;
```
```sql
--avg by country and city
SELECT
     a.country
	,a.city
    ,ROUND(AVG(a.productQuantity)::NUMERIC,2) AS Avg_Products_Ordered
FROM
    all_sessions a
WHERE
    productQuantity IS NOT NULL
    AND a.city <> 'Unknown'
	AND a.country <> 'Unknown'
GROUP BY
    a.country, a.city
ORDER BY
    Avg_Products_Ordered DESC;
```

Answer:
Spain and Madrid has the highest avg productquantity on all data, but that was also the only order so kind of an outlier in that sense.





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:








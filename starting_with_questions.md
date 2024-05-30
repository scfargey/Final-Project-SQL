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
```sql
--query by country first, look for patterns
    SELECT
         s.country
		,s.v2productcategory
        ,SUM(s.productQuantity * s.productprice) AS total_quantity_sold

    FROM
        all_Sessions s
    GROUP BY
        s.country, s.v2productcategory
	HAVING SUM(s.productQuantity * s.productprice) >0

	ORDER BY s.country, SUM(s.productQuantity * s.productprice) DESC

--query by city next, look for patterns
    SELECT
         s.city
		,s.v2productcategory
        ,SUM(s.productQuantity * s.productprice) AS total_quantity_sold

    FROM
        all_Sessions s

	WHERE s.city <> 'Unknown'

		
    GROUP BY
        s.city, s.v2productcategory
	
	HAVING SUM(s.productQuantity * s.productprice) >0

	ORDER BY s.city, SUM(s.productQuantity * s.productprice) DESC

```
Answer:
Home products seem to be the top selling products or near the top in all countries, and then the Home/Nest/Nest-USA is a top seller in most cities across the USA.




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```SQL
--calculate totals by city/country
WITH CTE_Totals AS (
    SELECT
        s.country
        ,s.city
        ,s.productsku
        ,s.v2productname
        ,SUM(s.productQuantity * s.productprice) AS total_sales
    FROM
        all_Sessions s
    WHERE 
        s.city <> 'Unknown'
        AND s.v2productcategory <> '(not set)'
    GROUP BY
        s.country
		,s.city
		,s.productsku
		,s.v2productname
    HAVING SUM(s.productQuantity * s.productprice) > 0
    ORDER BY s.country, s.city, SUM(s.productQuantity * s.productprice) DESC
),
--calculate ranks basedon those totals
RANKS AS (
    SELECT
         country
        ,city
        ,productsku
        ,v2productname
        ,total_sales
        ,RANK() OVER (PARTITION BY country, city ORDER BY total_sales DESC) AS ranks1
    FROM
        CTE_Totals
)
--only view the top sellers
SELECT
     country
    ,city
    ,productsku
    ,v2productname AS ProductName
    ,total_sales
    ,ranks1
FROM
    RANKS
WHERE
    ranks1 = 1
ORDER BY
    country,
    city;

```

Answer:
"country"	"city"	"productsku"	"productname"	"total_sales"	"ranks1"
"India"	"Bengaluru"	"GGOEGAAB033816"	"Google Men's Vintage Badge Tee Black"	16.99	1
"Ireland"	"Dublin"	"GGOEGBRB013899"	"Google Laptop Backpack"	99.99	1
"Spain"	"Madrid"	"GGOEWAEA083899"	"Waze Dress Socks"	89.9	1
"United States"	"Ann Arbor"	"GGOEGAAX0338"	"Google Men's Vintage Badge Tee Black"	18.99	1
"United States"	"Atlanta"	"GGOEGBJR018199"	"Reusable Shopping Bag"	44.8	1
"United States"	"Chicago"	"GGOEGHGR019499"	"Google Sunglasses"	2.8	1
"United States"	"Dallas"	"GGOEYOLR018699"	"YouTube Leatherette Notebook Combo"	6.99	1
"United States"	"Detroit"	"GGOEGDHC018299"	"Google 22 oz Water Bottle"	2.99	1
"United States"	"Houston"	"GGOEGAAX0037"	"Google Sunglasses"	7	1
"United States"	"Los Angeles"	"GGOEGAAX0362"	"Google Men's Pullover Hoodie Grey"	51.99	1
"United States"	"Mountain View"	"GGOENEBJ079499"	"Nest® Learning Thermostat 3rd Gen-USA - Stainless Steel"	398	1
"United States"	"New York"	"GGOENEBJ079499"	"Nest® Learning Thermostat 3rd Gen-USA - Stainless Steel"	249	1
"United States"	"Palo Alto"	"GGOENEBJ079499"	"Nest® Learning Thermostat 3rd Gen-USA - Stainless Steel"	149	1
"United States"	"San Francisco"	"GGOEGAAX0794"	"Nest® Learning Thermostat 3rd Gen-USA"	149	1
"United States"	"San Jose"	"GGOENEBB078899"	"Nest® Cam Indoor Security Camera - USA"	199	1
"United States"	"Seattle"	"GGOENEBB078899"	"Nest® Cam Indoor Security Camera - USA"	119	1
"United States"	"Sunnyvale"	"GGOENEBQ078999"	"Nest® Cam Outdoor Security Camera - USA"	199	1




**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
```SQL
WITH TotalRevenue AS (
    SELECT
        SUM(totalTransactionRevenue) AS total_revenue
    FROM
        all_Sessions
    WHERE
        city <> 'Unknown'
        AND v2productcategory <> '(not set)'
        AND totalTransactionRevenue > 0
),
CityCountryRevenue AS (
    SELECT
        country
        ,city
        ,SUM(totalTransactionRevenue) AS city_country_revenue
    FROM
        all_Sessions
    WHERE
        city <> 'Unknown'
        AND v2productcategory <> '(not set)'
        AND totalTransactionRevenue > 0
    GROUP BY
         country
		,city
)
SELECT
    c.country
    ,c.city
    ,c.city_country_revenue
    ,(c.city_country_revenue / tr.total_revenue) * 100 AS revenue_percentage
FROM
    CityCountryRevenue c
    CROSS JOIN TotalRevenue tr
ORDER BY
    revenue_percentage DESC;
```

Answer:

Find the total revenue number and rank by % of total sales






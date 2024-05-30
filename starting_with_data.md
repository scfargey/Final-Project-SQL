    - find all duplicate records
    - find the total number of unique visitors (`fullVisitorID`)
    - find the total number of unique visitors by referring sites
    - find each unique product viewed by each visitor
    - compute the percentage of visitors to the site that actually makes a purchase







Question 1:     - find all duplicate records

SQL Queries:
```SQL
--check for duplicated product names
SELECT
	p.productname
	,COUNT(p.productname)

FROM 
	products p

GROUP BY p.productname
```
---------------------------------------

```SQL
--check for all duplicated visitid's in the anlytics table, this table has no primary key

WITH DuplicateVisits AS (
    SELECT
        a.visitnumber,
        a.visitid,
        a.visitstarttime,
        a.sessiondate,
        a.fullvisitorid,
        a.userid,
        a.channelgrouping,
        a.socialengagementtype,
        a.units_sold,
        a.pageviews,
        a.timeonsite,
        a.bounces,
        a.revenue,
        a.unit_price,
        COUNT(*) OVER (PARTITION BY a.visitid) AS cnt
    FROM
        analytics a
)
SELECT
    visitnumber,
    visitid,
    visitstarttime,
    sessiondate,
    fullvisitorid,
    userid,
    channelgrouping,
    socialengagementtype,
    units_sold,
    pageviews,
    timeonsite,
    bounces,
    revenue,
    unit_price
FROM
    DuplicateVisits
WHERE
    cnt > 1
ORDER BY
    visitid, visitstarttime;
```

SELECT
Answer: 



Question 2: - find the total number of unique visitor (`fullVisitorID`)
```SQL
SQL Queries:
SELECT
	COUNT(DISTINCT a.fullvisitorid)
FROM
	analytics a

```
Answer:
119,952


Question 3: find the total number of unique visitors by referring sites

SQL Queries:
```SQL
SELECT
	a.channelgrouping
	,COUNT(DISTINCT a.fullvisitorid)
FROM
	analytics a
GROUP BY a.channelgrouping
```
Answer:
18,376



Question 4: find each unique product viewed by each visitor

SQL Queries:
```SQL
SELECT
	 p.SKU
	,p.productname
	,s.fullvisitorid
	,COUNT (s.fullvisitorid)


FROM 
	all_Sessions s
	INNER JOIN products p ON s.productsku = p.sku

GROUP BY 
	 p.SKU
	,p.productname
	,s.fullvisitorid
```

Answer:



Question 5: compute the percentage of visitors to the site that actually makes a purchase

SQL Queries:
```SQL

WITH TotalVisitors AS (
    SELECT 
        COUNT(DISTINCT visitid) AS total_visitors
    FROM 
        all_Sessions
),
VisitorsWithTransactions AS (
    SELECT 
        COUNT(DISTINCT visitid) AS visitors_with_transactions
    FROM 
        all_Sessions
    WHERE 
        transactions > 0
)
SELECT 
    (vw.visitors_with_transactions * 100.0 / tv.total_visitors) AS percentage_of_visitors_with_transactions
FROM 
    TotalVisitors tv,
    VisitorsWithTransactions vw;
```
Answer:
0.54960153888430887606


Question # 6 - customer who has visited the most?
```SQL

SELECT
    fullVisitorId
    ,COUNT(visitId) AS visit_count
FROM
    all_Sessions
GROUP BY
    fullVisitorId
ORDER BY
    visit_count DESC
LIMIT 1
```

Question # 7 - product with the most views?
```SQL
SELECT
     productsku
    ,v2productname
    ,SUM(pageviews) AS total_pageviews
FROM
    all_Sessions
GROUP BY
    productsku
	,v2productname
ORDER BY
    total_pageviews DESC
LIMIT 1;

```


Question# 8 - top 10 page views?
```SQL
SELECT
    productsku
    ,v2productname
    ,SUM(pageviews) AS total_pageviews
FROM
    all_Sessions
GROUP BY
    productsku
	,v2productname
ORDER BY
    total_pageviews DESC
LIMIT 10;
```



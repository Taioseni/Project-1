Question 1: find all duplicate records

SQL Queries: 
```
SELECT COUNT(visitid), visitid FROM all_sessions
	     GROUP BY visitid
	     HAVING COUNT(visitid) > 1
```

Answer: 553



Question 2: find the total number of unique visitors (`fullVisitorID`)

SQL Queries: 
```
SELECT COUNT(DISTINCT fullvisitorid) FROM all_sessions
```

Answer: 14223



Question 3: find the total number of unique visitors by referring sites

SQL Queries:
```
SELECT COUNT(DISTINCT fullvisitorid) FROM all_sessions
	     WHERE channelgrouping = 'Referral'
```
Answer: 2419



Question 4: find each unique product viewed by each visitor

SQL Queries:
```
SELECT DISTINCT productsku, fullvisitorid FROM temp_tab1
WHERE fullvisitorid IS NOT NULL
```

Answer: 1761



Question 5: compute the percentage of visitors to the site that actually makes a purchase

SQL Queries: 
```
SELECT CONCAT(ROUND(( (SELECT COUNT(fullvisitorid)*1.0 FROM all_sessions WHERE 	    
totaltransactionrevenue IS NOT NULL) 
(SELECT  COUNT(fullvisitorid) FROM all_sessions)*100),2), '%') AS Percentage
```

Answer: 0.54%


write 3 - 5 new questions that you could answer with this database. For each question, include
The queries you used to answer the question

Question 1: What country generated the least revenue?

SQL Queries: 
```
SELECT country, SUM(totaltransactionrevenue) AS Total_Revenue
FROM all_sessions
WHERE country IS NOT null AND totaltransactionrevenue IS NOT null
GROUP BY country
ORDER BY Total_Revenue ASC
LIMIT 1;
```

Answer: 16990000



Question 2: Can you provide a list of all visitors, including their city and country along with the total revenue generated from their transactions

SQL Queries: 
```
SELECT fullvisitorid, city, country, 
SUM(totaltransactionrevenue) AS totaltransactionrevenue
FROM all_sessions
GROUP BY fullvisitorid, city, country ORDER BY totaltransactionrevenue DESC;
```

Answer: United States



Question 3: How many sessions were recorded for each product SKU?

SQL Queries: 
```
SELECT alls.productsku, COUNT(alls.fullvisitorid) AS session_count
FROM all_sessions AS alls
JOIN sales_by_sku AS sbs ON alls.productsku = sbs.productsku
GROUP BY alls.productsku
ORDER BY session_count DESC;
```


Answer: 284 from GGOEGAAX0104


Question 4: What are the top-selling products in each city, ranked by the total quantity ordered, from the highest to the lowest?

SQL Queries: 
```
SELECT city, v2productname AS top_selling_product, 
SUM(orderedquantity) AS total_ordered_quantity
FROM temp_tab2
WHERE city IS NOT NULL GROUP BY city, v2productname
ORDER BY city, total_ordered_quantity DESC;
```

Answer: Mountain View
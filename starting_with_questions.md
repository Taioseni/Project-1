Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries: SELECT country, ROUND(SUM(totaltransactionrevenue),2) AS TotalRevCountry
		FROM all_sessions
		WHERE city IS NOT null AND totaltransactionrevenue IS NOT null
		GROUP BY country
		ORDER BY TotalRevCountry DESC
		LIMIT 5;

		SELECT city, country, ROUND(SUM(totaltransactionrevenue), 2) AS TotalRevCity
		FROM all_sessions
		WHERE city IS NOT null AND totaltransactionrevenue IS NOT null
		GROUP BY city, country
		ORDER BY TotalRevCity DESC
		LIMIT 5;



Answer: The top 5 countries with highest level of transaction revenues are: United state, Isreal, Australia, Canada, and Switzerland

The top 5 cities with the highest level of transactions are: San Francisco, Sunnyvale, Atlanta, Pale Alto and Tel Aviv-Yafo




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries: SELECT country, ROUND(AVG(units_sold), 2) AS avg_qty_country
		FROM temp_tab1
		WHERE units_sold IS NOT NULL
		GROUP BY country
		ORDER BY avg_qty_country DESC
		LIMIT 5


		SELECT city, country, ROUND(AVG(units_sold), 2) AS avg_qty_city
		FROM temp_tab1
		WHERE units_sold IS NOT NULL AND city IS NOT null 
		GROUP BY city, country 
		ORDER BY avg_qty_city DESC
		limit 5



Answer: The country with the average number of products orders are; United States, Canada, Hong-Kong, Germany, and Ireland


The cities with the average number of product ordered are; Mountain View, New York, Sunnyvale, Jersey City and Paris all in the United States



**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries: SELECT country, v2productcategory, COUNT(v2productcategory) AS prd_cat_country
		FROM temp_tab1
		WHERE country IS NOT null  
		GROUP BY country, v2productcategory
		ORDER BY prd_cat_country DESC


	SELECT city, country, v2productcategory, COUNT(v2productcategory) AS prd_cat_city
	FROM temp_tab1
	WHERE city IS NOT null  
	GROUP BY city, country, v2productcategory
	ORDER BY prd_cat_city DESC


Answer: The pattern in the types of product categories ordered from visitors in different countries showed the United States ordering first top 25 and subsequently

The pattern in the types of product categories ordered from visitors in different cities showed the United States with highest numbers of products ordered via different product categories





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries: WITH CTE AS (SELECT country, v2productname AS top_selling_product, 		SUM(orderedquantity) AS max_qty_country
FROM temp_tab2
WHERE country IS NOT null AND orderedquantity IS NOT NULL
GROUP BY country, v2productname
ORDER BY max_qty_country DESC),
RankedTable AS(
SELECT country, top_selling_product, max_qty_country,
RANK() OVER (PARTITION BY country ORDER BY max_qty_country DESC) AS rannk_ FROM CTE)
SELECT * FROM RankedTable WHERE rannk_ = 1

WITH CTE AS (SELECT city, country, v2productname AS top_selling_product, SUM(orderedquantity) AS max_qty_city
FROM temp_tab2
WHERE city IS NOT NULL AND country IS NOT null AND orderedquantity IS NOT NULL
GROUP BY city, country, v2productname
ORDER BY max_qty_city DESC),
RankedTable AS(
SELECT city, country, top_selling_product, max_qty_city,
RANK() OVER (PARTITION BY city ORDER BY max_qty_city DESC) AS rannk_ FROM CTE)
SELECT * FROM RankedTable WHERE rannk_ = 1


Answer: The top selling product from each country are from country not set - YouTube custom decals, Albania with 22 oz YouTube bottle infuser, Algeria YouTube twill can, Argentina SPF-15 Slim & Sslender lip balm etc

The top selling product from each cities/countries are; Adelaide Australia with google mens watershed full zip hoodie, Ahmedabad India with leather notebook combo.
The pattern worthy of noting from the product sold was that they were majorly sold on line through different platforms- Youtube & google



**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries: SELECT country, SUM(totaltransactionrevenue) AS Total_Sum1 
FROM all_sessions
WHERE city IS NOT null AND totaltransactionrevenue IS NOT null
GROUP BY country 
ORDER BY Total_Sum1 DESC

SELECT country, MAX(totaltransactionrevenue) AS Total_Max1
FROM all_sessions
WHERE city IS NOT null AND totaltransactionrevenue IS NOT null
GROUP BY country 
ORDER BY Total_Max1 DESC

SELECT country, MIN(totaltransactionrevenue) AS Total_Min1
FROM all_sessions
WHERE city IS NOT null AND totaltransactionrevenue IS NOT null
GROUP BY country 
ORDER BY Total_Min1 DESC

SELECT country, AVG(totaltransactionrevenue) AS Avg_Total1
FROM all_sessions
WHERE city IS NOT null AND totaltransactionrevenue IS NOT null
GROUP BY country 
ORDER BY Avg_Total1 DESC


Answer: Impact of revenue generated is broken down below
1 the flow of revenue generated by countries from the first query is United States, Isreal, Australia, Canada and Switzerland

2. Impact of revenue generated by countries to the total maximum revenue are; united States, Isreal, Australia, Canada and Switzerland

3. Impact of revenue generated by countries to the total minimum revenue are; Isreal, Australia, Canada, Switzerland and United States

4. Impact of revenue generated by countries to the average minimum revenue are; Isreal, Australia, Canada, Switzerland and United States

Cities with the max Sum transactions revenue are; San Francisco, Sunnyvale, Atlanta, Palo Alto etc

Cities with the max total total transactions revenue are; Atlanta, Sunnyvale, Tel-Aviv_Yafo, Los Angeles and Sydney

Cities with the max Min total transactions revenue are; Tel-Aviv_Yafo,, Sydney, Seattle, Nashville and Palo Alto

Cities with the average total transactions revenue are; Tel-Aviv-Yafo, Atlante, Sydney, Seattle and Sunnyvale










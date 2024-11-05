Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

SELECT city, country, transaction_revenue
FROM all_sessions
WHERE transaction_revenue IS NOT NULL
ORDER BY transaction_revenue


Answer: not many data is avaibale as transaction_revenue in most cases is null. Few rows have transaction_revenue
available for which only one city name is available. The country with highest transaction revenue is USA.




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

SELECT city, country, FLOOR(AVG(product_quantity)) AS avg_product_ordered
FROM all_sessions
GROUP BY city, country
HAVING AVG(product_quantity) IS NOT NULL

Answer:

Many rows have NULL values in product_quantity, which affects the final results by producing averages that are NULL. To address this, I added HAVING AVG(product_quantity) IS NOT NULL to filter out rows where the average product ordered is NULL. Additionally, some rows lack city names, which leads to incomplete data, making the results less reliable for decision-making or analysis.




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
-- Cleaning
UPDATE all_sessions
SET v2_product_category = '(not set)'
WHERE v2_product_category = '${escCatTitle}'

SELECT city, country, v2_product_category AS product_category, COUNT(*) AS number_of_orders_per_category
FROM all_sessions
WHERE v2_product_category != '(not set)' AND city != '(not set)' AND country != '(not set)'
GROUP BY city, country, v2_product_category
ORDER BY number_of_orders_per_category DESC;

Answer:
Again, some cities are not specified in the database. but we can see that most people in the US order Home/Shop by Brand/YouTube/ and Home/Apparel/Men's/Men's-T-Shirts/.




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:








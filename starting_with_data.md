Question 1: What are the top 5 most frequently viewed products on the site?

SQL Queries:

SELECT v2_product_name AS product, COUNT (*) AS views
FROM all_sessions
GROUP BY v2_product_name
ORDER BY views DESC
LIMIT 5

Answer: 
"Google Men's 100% Cotton Short Sleeve Hero Tee White"	with 295 views
"22 oz YouTube Bottle Infuser"	with 245 views
"YouTube Twill Cap"	 with 221 views
"YouTube Custom Decals"	with 211 views
"YouTube Men's Short Sleeve Hero Tee Black"	with 197 views


Question 2: 

SQL Queries:

Answer:



Question 3: 

SQL Queries:

Answer:



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:

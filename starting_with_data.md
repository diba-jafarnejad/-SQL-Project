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

This allows the company to see which products are best sellers and people have most interest in.


Question 2: How many repeat visitors are there, and which city/country do they come from?

SQL Queries: 
SELECT count(*) AS total_number_of_repeated_visitors
FROM (
SELECT full_visitor_id, city, country, COUNT (full_visitor_id) AS visits
FROM all_sessions
GROUP BY full_visitor_id, city, country
HAVING COUNT (full_visitor_id) > 1
ORDER BY full_visitor_id ASC)

Answer:
The inner subqueries returns the city, country and the visitor id and number of visits they made. The entire query returns 773 which means total of 773 visitors have visited the site again.

To check the acurracy of this query, I ran one of the result to see if the result is correct.

-- to check the answer is right:
SELECT full_visitor_id, city, country
FROM all_sessions
WHERE full_visitor_id = 5308587084747813

Question 3: 

SQL Queries:

Answer:



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:

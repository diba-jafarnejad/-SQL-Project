Question 1: What are the top 5 most frequently viewed products on the site?

SQL Queries:

```
SELECT v2_product_name AS product, COUNT (*) AS views
FROM all_sessions
GROUP BY v2_product_name
ORDER BY views DESC
LIMIT 5
```
Answer: 
"Google Men's 100% Cotton Short Sleeve Hero Tee White"	with 295 views
"22 oz YouTube Bottle Infuser"	with 245 views
"YouTube Twill Cap"	 with 221 views
"YouTube Custom Decals"	with 211 views
"YouTube Men's Short Sleeve Hero Tee Black"	with 197 views

This allows the company to see which products are best sellers and people have most interest in.


Question 2: How many repeat visitors are there, and which city/country do they come from?

SQL Queries: 
```
-- the main query counts the number of repeated visitors 
SELECT count(*) AS total_number_of_repeated_visitors
-- this inner query retrieves rows of repeated visitors
FROM (
SELECT full_visitor_id, city, country, COUNT (full_visitor_id) AS visits
FROM all_sessions
GROUP BY full_visitor_id, city, country
HAVING COUNT (full_visitor_id) > 1
ORDER BY full_visitor_id ASC)
```
Answer:
The inner subqueries returns the city, country and the visitor id and number of visits they made. The entire query returns 773 which means total of 773 visitors have visited the site again.

To check the acurracy of this query, I ran one of the result to see if the result is correct.

```
-- to check the answer is right:
SELECT full_visitor_id, city, country
FROM all_sessions
WHERE full_visitor_id = 5308587084747813
```

Question 3: What is the average time spent on the site by visitors from each city?

SQL Queries:

```
SELECT city, country, (AVG(time_on_site) / 60)::numeric(10, 2) AS average_time_in_minutes
FROM all_sessions
GROUP BY city, country
HAVING (AVG(time_on_site) / 60)::numeric(10, 2) IS NOT NULL
ORDER BY average_time_in_minutes DESC
```

Answer:
some of the data in the time column is null - to clean the data added the HAVING condition.
And to make it more readble, I changed the seconds to mins
Visitors from Japan have the most average time spent on the site with 39.70 minutes.


Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:

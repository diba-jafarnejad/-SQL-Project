What are your risk areas? Identify and describe them.



QA Process:
Describe your QA process and include the SQL queries used to execute it.


### 1. Data Completeness and Consistency:

Missing, inconsistent, or incomplete data can lead to skewed or inaccurate results, particularly if key fields like city, country, or v2_product_category are not consistently populated.

Missing values in many columns such as cities and revenue

```
-- updating all_session as follow to address inconsistency
UPDATE all_sessions
SET v2_product_category = '(not set)'
WHERE v2_product_category = '${escCatTitle}'

```

```
-- To calculate the average for revenue, had to set revenue to 0 instead of null value
UPDATE analytics
SET revenue = 0
WHERE revenue IS NULL;

```

---
### 2. Duplicate Records: 

Duplicate records can inflate counts or revenue metrics, leading to inaccurate insights and issues definfing primary keys. For example, table Analytics had a duplicated row which I removed.

```
-- removing duplicates in the analytics table:
CREATE INDEX IF NOT EXISTS idx_analytics_partition ON analytics (full_visitor_id, visit_id, visit_start_time, unit_price, revenue);

CREATE TEMP TABLE temp_duplicates AS
WITH ranked_rows AS (
    SELECT 
        ctid AS row_id,
        ROW_NUMBER() OVER (
            PARTITION BY full_visitor_id, visit_id, visit_start_time, unit_price, revenue
            ORDER BY ctid
        ) AS row_num
    FROM 
        analytics
)
SELECT 
    row_id
FROM 
    ranked_rows
WHERE 
    row_num > 1;

DELETE 
FROM 
    analytics
WHERE 
    ctid IN (SELECT row_id FROM temp_duplicates);

DROP TABLE IF EXISTS temp_duplicates;

```

---
### 3. Data Type Mismatches: 

Many datas entered have no information on the unit of measurements such as revenue and time which can lead to issues in calculations, aggregations, or comparisons. For example, the time on site! I assumed its in seconds but the data does not provide additional information on this. maybe it would be better to name the columns better for understanding.

```
-- converting the unit price to make it more readable and understandable
UPDATE analytics
SET unit_price = unit_price / 1000000
WHERE unit_price IS NOT NULL;

```

```
-- converting time to minutes for better readblity and understanding
SELECT city, country, (AVG(time_on_site) / 60)::numeric(10, 2) AS average_time_in_minutes
FROM all_sessions
GROUP BY city, country
HAVING (AVG(time_on_site) / 60)::numeric(10, 2) IS NOT NULL
ORDER BY average_time_in_minutes DESC
```

---
### 4. Outliers and Extreme Values: 

Extreme values, such as unusually high time_on_site or revenue, can skew averages and lead to misleading conclusions.

```
WITH average_time AS (
    SELECT 
        city, 
        country, 
        (AVG(time_on_site) / 60)::numeric(10, 2) AS average_time_in_minutes
    FROM 
        all_sessions
    GROUP BY 
        city, country
    HAVING 
        (AVG(time_on_site) / 60)::numeric(10, 2) IS NOT NULL
)
SELECT 
    AVG(average_time_in_minutes) AS mean,
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY average_time_in_minutes) AS median,
    STDDEV(average_time_in_minutes) AS stddev,
	PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY average_time_in_minutes) AS percentile_25,
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY average_time_in_minutes) AS median,
    PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY average_time_in_minutes) AS percentile_75,
    PERCENTILE_CONT(0.9) WITHIN GROUP (ORDER BY average_time_in_minutes) AS percentile_90,
    PERCENTILE_CONT(0.95) WITHIN GROUP (ORDER BY average_time_in_minutes) AS percentile_95,
    MAX(average_time_in_minutes) AS max_value
FROM 
    average_time;
```
We see extreme values - skewed to the right percentile 50(median) is extremly lower than percentile 90 and high outliers

--- 
### 5. SQL Query Errors or Logical Mistakes: 

Incorrect SQL syntax or logical errors can produce incorrect results, particularly in complex calculations or multi-table joins. I wrote test queries to make sure the results are correct.
In removing duplicates in the analytics table, I also ran the following query to make sure the duplicated data is removed.
```
SELECT *
FROM analytics
WHERE full_visitor_id = 118367773912785787 
  AND visit_id = 1496536443 
  AND visit_start_time = 1496536443 
  AND unit_price = 2800000 
  AND revenue = 4133333;

```


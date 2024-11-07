What are your risk areas? Identify and describe them.



QA Process:
Describe your QA process and include the SQL queries used to execute it.
###Data Completeness and Consistency:
Missing, inconsistent, or incomplete data can lead to skewed or inaccurate results, particularly if key fields like city, country, or v2_product_category are not consistently populated.

---
###Missing values in many columns such as cities and revenue
Duplicate Records: Duplicate records can inflate counts or revenue metrics, leading to inaccurate insights and issues definfing primary keys. For example, table Analytics had a duplicated row which I removed.

---
###Data Type Mismatches: many datas entered have no information on the unit of measurements such as revenue and time which can lead to issues in calculations, aggregations, or comparisons. For example, the time on site! I assumed its in seconds but the data does not provide additional information on this. maybe it would be better to name the columns better for understanding.

---
###Outliers and Extreme Values: Extreme values, such as unusually high time_on_site or revenue, can skew averages and lead to misleading conclusions.
SQL Query Errors or Logical Mistakes: Incorrect SQL syntax or logical errors can produce incorrect results, particularly in complex calculations or multi-table joins. I wrote test queries to make sure the results are correct.

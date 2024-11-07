What issues will you address by cleaning the data?

### 1. Handling Duplicated Rows:

Issue: The analytics table contained exact duplicate rows, which prevented establishing primary and foreign keys. Duplicate rows could lead to inflated metrics and incorrect analysis.

Solution: We used ROW_NUMBER() and the ctid column (a unique identifier for each row) to identify and delete duplicates. An index was also created to speed up this process, but the query was initially slow due to the lack of primary keys.

---
### 2. Managing Null Values in revenue:

Issue: The revenue column contained NULL values, which could cause calculation errors or inconsistencies in reports.

Solution: Updated all NULL values in revenue to 0 to maintain consistency and ensure that revenue-based calculations are accurate.

---
### 3. Correcting Units in unit_price:

Issue: The unit_price column values were not stored in the correct unit (assumed to be in millions), which could cause misunderstandings in price-based analysis.

Solution: Converted unit_price to the correct unit by dividing it by 1,000,000, ensuring consistent and accurate analysis.

---
### 4. Setting Default Value in v2_product_category:

Issue: Missing values in the v2_product_category column could lead to gaps in category-based analysis and decision-making.

Solution: Set a default value of '(not set)' for missing categories to ensure that the column is consistently populated and can be analyzed more accurately.



Queries:
Below, provide the SQL queries you used to clean your data.

### 1. Removing Duplicate Rows in Analytics table:
```
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
To verify the duplicates are removed:
```
SELECT *
FROM analytics
WHERE full_visitor_id = 118367773912785787 
  AND visit_id = 1496536443 
  AND visit_start_time = 1496536443 
  AND unit_price = 2800000 
  AND revenue = 4133333;
```

### 2. Removing NULL Values in revenue
```
UPDATE analytics
SET revenue = 0
WHERE revenue IS NULL;

```

### 3. Updating unit_price to correct Unit
```
UPDATE analytics
SET unit_price = unit_price / 1000000
WHERE unit_price IS NOT NULL;

```

### 4. Setting Default Value for Missing Categories in all_sessions
```
UPDATE all_sessions
SET v2_product_category = '(not set)'
WHERE v2_product_category = '${escCatTitle}';

```

#### What is SQL

-   SQL (Structured Query Language) is a programming language used for managing, manipulating, and analyzing data in relational databases.

#### Relational Database

-   Relational databases use a relational model of tables to organize data.
-   Tables consist of rows (records) and columns (attributes).
-   Tables tend to adhere to a schema which defines the table structure.
-   Tables are connected together with keys (i.e. user_id persists across tables).

#### Primary Key

-   A table column or set of columns that uniquely identifies a table row/record.
-   Database vendors have different rules for primary keys (i.e. single column constraints, NULL value handling, etc).

#### Foreign Key

-   A table column or set of columns that refer to a primary key in another table.
-   A table can have multiple foreign keys.

#### Different SQL dialects

-   SQL dialects vary slightly between database systems (e.g. PostgreSQL, Amazon Redshift, Google BigQuery). Like flavors of ice cream.

-   Each system might have unique functions, syntax, or performance optimizations.

#### ETLs

-   Database management jobs used to Extract, Transform, and Load data from various sources into a database.
-   i.e. Extract data from a 3rd party API, Transform the data (clean, arrange, format), Load the data into database.
-   ETLs can store results of an analyst query in a table to help with speed to access data output and code visibility.
-   ETLs are typically scheduled to run and write data to X table on a set frequency.
-   Often recommended for analysts to wait till code is stable (passed rapid iteration) before moving to ETL.

#### CRUD (Create, Read, Update, Delete)

-   Common operation themes in database management.
-   Typically owned by data engineers.
-   However, useful for analysts to be aware of these operations when writing ETL code/logic.

#### Creating Tables

-   Creste table syntax.

```         
CREATE TABLE users ( 
    user_id INT PRIMARY KEY,
    first_name text,
    birth_year INT,
    favorite_color text
);
```

#### Typical Query Outline to Read Data from Table

-   Read table syntax.
-   When querying/exploring a table, it is best practice to include LIMIT logic to reduce database load.

```         
SELECT 
    <specify columns/metrics>
FROM 
    <table_name>
WHERE 
    <filter conditions>
GROUP BY 
    <groups to group/aggregate by>
HAVING 
    <post process filter>
ORDER BY 
    <column(s) to order by>
    <column to order small to larger> ASC,  
    <column to order large to small> DESC 
LIMIT 
    <view limited number of rows vs full query output; use for iterative exploration>
```

#### Typical Query Evaluation Order

-   Queries may have 1 or more of these eval elements
-   FROM (calling table and join logic)
-   WHERE (picking up any filters)
-   GROUP BY (aggregation grain)
-   HAVING (filter applied to agg functions)
-   WINDOW FUNCTIONS (logic for functions applied across a window in the data)
-   SELECT (specified columns return/functions to call)
-   DISTINCT (remove dups)
-   UNION (combine result sets)
-   ORDER BY (arrange output data)
-   LIMIT (constrain the number of records returned)

#### Add Records to Existing Table

-   Insert table syntax.

```         
WITH user_staging AS (
SELECT
    123 AS user_id,
    'betty' AS first_name,
    1965 AS birth_year,
    'red' AS favorite_color
    
UNION

SELECT
    456 AS user_id,
    'bill' AS first_name,
    1981 AS birth_year,
    'green' AS favorite_color
)

INSERT INTO users (user_id, first_name, birth_year, favorite_color)
SELECT user_id, first_name, birth_year, favorite_color
FROM user_staging;
```

#### Add Columns to Existing Table

-   Adding columns syntax.

```         
ALTER TABLE users
ADD COLUMN annual_income_usd INT,
ADD COLUMN column_drop BOOLEAN;
```

-   Removing columns syntax.

```         
ALTER TABLE users
DROP COLUMN column_drop;
```

#### Conditionally Update Records

-   Update syntax.

```         
UPDATE users
SET annual_income_usd = 50000;
```

#### CTEs (Common Table Expression)

-   Temporary named result that can be referenced within the same query.
-   Often used for simplicity/readability/efficiency.
-   CTEs can help break complex logic into manageable chunks for QA/iteration and potential query performance improvement.
-   WITH keyword used to define first CTE in a query.

```         
-- TODO: update with improved formatting 
WITH us_states_codes AS (
    SELECT 
        UNNEST(
            ARRAY[
                'WY','WI','WV','WA','VA','VT','UT','TX','TN','SD','SC',
                'RI','PA','OR','OK','OH','ND','NC','NY','NM','NJ','NH',
                'NV','NE','MT','MO','MS','MN','MI','MA','MD','ME','LA',
                'KY','KS','IA','IN','IL','ID','HI','GA','FL','DE','CT',
                'CO','CA','AR','AZ','AK','AL'
            ]   
        ) AS state_codes
    
)
SELECT * FROM us_states_codes
```

#### TEMP tables

-   Another approach to store temporary results.
-   Temp tables are dropped at the end of a session but can be referenced within a session.
-   Temp tables are created using similar logic to non-temp tables.

```         
-- DROP TABLE IF EXISTS top_20_quarterbacks;
CREATE TEMP TABLE top_20_quarterbacks (
    id SERIAL PRIMARY KEY,
    player_name VARCHAR(255),
    team VARCHAR(255),
    total_td INTEGER
);

-- source: https://www.statmuse.com/nfl/ask/nfl-qb-total-touchdown-leaders-2022
INSERT INTO top_20_quarterbacks (player_name, team, total_td)
VALUES ('Patrick Mahomes', 'KC', 45),
       ('Josh Allen', 'BUF', 42),
       ('Joe Burrow', 'CIN', 40),
       ('Jalen Hurts', 'PHI', 35),
       ('Kirk Cousins', 'MIN', 31),
       ('Geno Smith', 'SEA', 31),
       ('Trevor Lawrence', 'JAX', 30),
       ('Jared Goff', 'DET', 29),
       ('Aaron Rodgers', 'GB', 27),
       ('Tom Brady', 'TB', 26),
       ('Justin Herbert', 'LAC', 25),
       ('Tua Tagovailoa', 'MIA', 25),
       ('Justin Fields', 'CHI', 25),
       ('Derek Carr', 'LV', 24),
       ('Dak Prescott', 'DAL', 24),
       ('Daniel Jones', 'NYG', 22),
       ('Lamar Jackson', 'BAL', 20),
       ('Russell Wilson', 'DEN', 19),
       ('Marcus Mariota', 'ATL', 19),
       ('Davis Mills', 'HOU', 19);
       
-- generate summary stats from temp table
SELECT
    AVG(total_td::FLOAT) AS avg_total_tds_top_20_qb,
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY total_td) AS median_total_tds_top_20_qb
FROM top_20_quarterbacks
```

#### Subquiries (aka nested queries)

-   Often useful for simple filtering conditions in WHERE clause, adding a column to query, or temp result in a query.
-   Note: complex subquiries can make readability difficult.
-   CTEs or TEMP tables tend to be more readable for complex logic.

```         
-- generate sample data CTE
WITH employees(name, department, salary) AS (
    VALUES
    ('John', 'HR', 50000),
    ('Sarah', 'HR', 60000),
    ('Steve', 'Sales', 70000),
    ('Jake', 'Sales', 80000),
    ('Laura', 'IT', 55000),
    ('Mike', 'IT', 65000),
    ('Emily', 'Marketing', 75000),
    ('Paul', 'Marketing', 85000)
)

-- return employee info for folks with above average salary
SELECT 
  name, 
  department, 
  salary
FROM employees
-- subquery in the where clause to assist with filtering
WHERE salary > (SELECT AVG(salary) FROM employees)
```

#### Handling NULL values

-   NULL in SQL represents the absence of a value.
-   By default, most aggregate functions ignore NULL values.
-   WHERE clause comparison operators (=, \<, \>, etc) on NULL values do not get evaluated resulting in records getting dropped.
-   Said differently, comparison operators do not work with NULL values as NULL values represent the absence of a value so a comparison is not feasible.
-   Use **`IS NULL`** or **`IS NOT NULL`** to include or exclude NULL values in queries.
-   In some cases, we may want to replace NULLs with another value (numerous techniques exist for NULL replacement).

```         
WITH pet_goats AS (
    SELECT 'becky' AS name, 4 AS pet_goats
        UNION
    SELECT 'jill' AS name, 2 AS pet_goats
        UNION
    SELECT 'becky' AS name, NULL AS pet_goats
        UNION
    SELECT 'tom' AS name, 1 AS pet_goats
)

-- returns tom and jill
SELECT name FROM pet_goats WHERE pet_goats <= 2

-- returns tom, jill, and becky
-- include NULL records approach 1
SELECT name FROM pet_goats WHERE (pet_goats <= 2 OR pet_goats IS NULL)

-- returns tom, jill, and becky
-- include NULL records approach 2
SELECT name FROM pet_goats WHERE COALESCE(pet_goats, 0) <= 2

-- WARNING: equal comparison operator does not work as one might expect with NULL values; use IS NULL
-- no records returned
SELECT name FROM pet_goats WHERE pet_goats = NULL

-- returns becky only
SELECT name FROM pet_goats WHERE pet_goats IS NULL

-- returns all names expect becky
SELECT name FROM pet_goats WHERE pet_goats IS NOT NULL
```

#### Casting

-   Conversion of a value of one data type to another data type.
-   CAST function, double colon, or to_datatype functions to convert/cast one data type to another.
-   Sidebar: Amazon Redshift defaults to integer division which drops decimal places in output. Cast to float to include decimal places.

```         
-- Converts the string '123' to an integer 123
SELECT '123'::integer; 

-- Does the same thing as the previous example
SELECT CAST('123' AS integer); 

-- cast logical/boolean data type to integer (0 if FALSE, 1 if TRUE)
SELECT 
    CAST(FALSE AS INTEGER) AS binary_flag_0,
    CAST(TRUE AS INTEGER) AS binary_flag_1;

-- cast string to date using double colon syntax
SELECT '2022-12-31'::DATE;

-- can also use the to_date function
SELECT to_date('2022-12-31', 'YYYY-MM-DD');

-- cast string to numeric
SELECT '123.45'::NUMERIC;

-- cast timestamp string to timestamp
SELECT to_timestamp('2023-12-31 14:30:00', 'YYYY-MM-DD HH24:MI:SS');

-- convert numeric to dollar currency format with commas as thousands separators
SELECT 
    to_char(1234567.89, '$9,999,999.99') As example_1,
    to_char(5033.41, '$9,999,999.99') As example_2;
```

#### CASE WHEN

-   If then boolean conditional logic.
-   Can be used to replace values in a column, create a new column, bucket data based on rules, etc.
-   CASE WHEN logic output data type determined by the THEN expression data types.

```         
-- see above to generate us_states_codes CTE
SELECT
    state_codes,
    CASE
        WHEN state_codes IN ('HI', 'AK', 'AZ', 'NM', 'OK')
        THEN TRUE 
        ELSE FALSE
    END AS joined_usa_after_1900
FROM us_states_codes
ORDER BY joined_usa_after_1900 DESC 
```

#### COALESCE

-   Replace NULL values with first non-NULL value.

```         
WITH example_record AS (
  SELECT
      NULL AS street_address,
      NULL AS neighborhood,
      'New York' AS city,
      'New York County' AS county,
      'New York' AS state
)

SELECT
    COALESCE(
        street_address, 
        neighborhood, 
        city, 
        county, 
        state, 
        'no_geo_info'
    ) AS most_granular_geo_result,
    CASE 
      WHEN street_address IS NOT NULL THEN 'street_address'
      WHEN neighborhood IS NOT NULL THEN 'neighborhood'
      WHEN city IS NOT NULL THEN 'city'
      WHEN county IS NOT NULL THEN 'county'
      WHEN state IS NOT NULL THEN 'state'
      ELSE 'no_geo_info'
   END AS most_granular_geo_info,
    *
FROM example_record
```

#### NULLIF

-   Dedicated function to assign NULL values based on conditional logic.

```         
WITH survey_responses AS (
SELECT 
    UNNEST(
            ARRAY[
                'I love this product',
                '',
                'Home page load is slow',
                'Not able to find profile settings'
            ]   
        ) AS survey_open_text_feedback_response
)

-- when survey response = '' then set value to NULL
SELECT 
    NULLIF(
        survey_open_text_feedback_response, 
        ''
    ) AS survey_open_text_feedback_response_clean
FROM survey_responses;
```

#### LEAST, GREATEST

-   Given multiple inputs and return min (least) or max (greatest) value.

GREATEST examples

```         
SELECT GREATEST(123, 500, 999, 10000, 1) AS largest_int;
SELECT GREATEST('2021-01-01', '2021-02-15', '2022-04-30') AS latest_date;
-- ASCII code used for each letter
SELECT GREATEST('a', 'b', 'c') AS largest_letter;
```

LEAST example

```         
WITH customer_spending AS (
SELECT 56987 AS customer_spending
UNION 
SELECT 46987 AS customer_spending
)

SELECT 
    LEAST(customer_spending, 50000) AS customer_spending_capped_50k
FROM customer_spending
```

#### DISTINCT

-   Return unique column values or row permutations.

```         
WITH us_customers AS (
    SELECT 123 AS user_id, 'FL' AS state, 'Orlando' as city
    UNION 
    SELECT 456 AS user_id, 'CA' AS state, 'San Jose' as city
    UNION 
    SELECT 789 AS user_id, 'GA' AS state, 'Atlanta' as city
    UNION 
    SELECT 101 AS user_id, 'GA' AS state, 'Atlanta' as city
)

-- return all unique city and state pairs
SELECT 
    DISTINCT
    state,
    city
FROM us_customers

---- sidebar: GROUP BY logic below can also be used to get unique combos and drop dup rows
-- SELECT
--  state,
--  city
-- FROM us_customers
-- GROUP BY 1,2
```

#### DISTINCT ON

-   Similar to DISTINCT except one or more columns are specified to grab unique values vs full row uniqueness.
-   For large tables, row_number window function with down stream filter tends to be more efficient; DISTINCT ON requires heavy table sort.

```         
WITH purchases AS (
    SELECT 123 AS user_id, 1 AS order_id, 100 AS purchase_amount
    UNION 
    SELECT 123 AS user_id, 2 AS order_id, 200 AS purchase_amount
    UNION 
    SELECT 456 AS user_id, 3 AS order_id, 444 AS purchase_amount
    UNION 
    SELECT 789 AS user_id, 4 AS order_id, 10 AS purchase_amount
)

-- grab first purchase by user
SELECT 
    DISTINCT ON (user_id) 
    user_id, 
    order_id, 
    purchase_amount
FROM purchases
ORDER BY user_id, order_id
```

#### Log Transformations

-   Common way to understand and visualize data that spans several orders of magnitude.
-   Log base 2 and log base 10 tend to be most intuitive.
-   Change of 1 unit on log2 scale = data value doubling on the original scale.
-   Change of 1 unit on log10 scale = data value 10xing on the original scale.

```         
WITH example_data(income) AS (
  VALUES 
    (100::numeric), (200), (300), (1000), (2000), (3000), (10000), (20000), (30000), (100000),
    (200000), (300000), (1000000), (2000000), (3000000), (10000000), (20000000), (30000000)
)

SELECT 
    income,
    LOG(2, income) as log_base_2_income,
    LOG(10, income) as log_base_10_income
FROM example_data
ORDER BY income
```

#### MOD

-   Calculates the whole number remainder when one number is divided by another number.
-   i.e. mod(10,2) = 0 vs mod(11,2) = 1
-   Useful for bucketing/sampling with reproducibility on id fields generated randomly.

Control vs variant bucketing example

```         
WITH users(user_id) AS (
  VALUES 
    (1), (2), (3), (4), (5), 
    (6), (7), (8), (9), (10), 
    (11), (12), (13), (14), (15), 
    (16), (17), (18), (19), (20) 
)

SELECT 
    user_id,
    user_id % 2 AS modulo_remainder,
    CASE 
      -- even user_ids get bucketed in control
      -- odd user_ids get bucketed in variant
      WHEN user_id % 2 = 0 THEN 'Control'
      ELSE 'Variant'
    END AS group_assignment
FROM users
```

Sample 10% of users

```         
-- generate 100 random user_ids
WITH users AS (SELECT 
    UNNEST(
        ARRAY(
          SELECT id
          FROM (SELECT generate_series(1000, 5000) AS id) s
          ORDER BY RANDOM()
          LIMIT 100
        )
    ) AS user_id
)

-- pull ~10% of user_ids as a sample
-- one approach using modulo math
SELECT
    user_id
FROM users
WHERE user_id % 10 = 1
```

#### Bucketing/bins: using NTILE

-   NTILE: divides an ordered partition into N roughly equal buckets.

```         
DROP TABLE IF EXISTS temp_sales;
CREATE TEMP TABLE temp_sales (
  id SERIAL PRIMARY KEY,
  item_name VARCHAR(255),
  price NUMERIC(10, 2)
);

INSERT INTO temp_sales (item_name, price)
VALUES ('Item A', 10.0),
       ('Item B', 20.0),
       ('Item C', 30.0),
       ('Item D', 40.0),
       ('Item E', 50.0),
       ('Item F', 60.0),
       ('Item G', 70.0),
       ('Item H', 80.0),
       ('Item I', 90.0),
       ('Item J', 100.0);

WITH sales_with_bins AS (
  SELECT *,
         ntile(4) OVER (ORDER BY price) AS ntile_bin
  FROM temp_sales
)

SELECT 
    id,
    item_name,
    price,
    ntile_bin,
    CASE
       WHEN ntile_bin = 1 THEN 'Low'
       WHEN ntile_bin = 2 THEN 'Medium Low'
       WHEN ntile_bin = 3 THEN 'Medium High'
       WHEN ntile_bin = 4 THEN 'High'
    END AS price_category
FROM sales_with_bins;
```

#### BETWEEN

-   Used to filter on a range of values ( x BETWEEN low_value and high_value).
-   The range is inclusive meaning x \>= to low_value and x \<= high_value.
-   To create an exclusive range use \> and \<.
-   Remember that NULLs are dropped by default; to include NULLs use (x is NULL or x BETWEEN ...).
-   Between dates can be tricky and folks may unexpectedly drop data.

```         
-- imagine ts_var is a date timestamp
-- this filter would not include a full days worth of data for last day of Jan
ts_var BETWEEN '2023-01-01' AND '2023-01-31' 

-- could be a better option
ts_var BETWEEN '2023-01-01' AND '2023-02-01' 

-- personally perferrred is the following
ts_var >= '2023-01-01' AND < '2023-02-01' 
```

#### Pivoting data

-   Pivoting complex data from narrow to wide or wide to narrow can be tricky with most SQL database vendors.
-   Tools outside of SQL are often preferred if heavy pivoting of data is needed.
-   Manual/hard coded solutions exist within PostgreSQL.
-   BigQuery has a PIVOT function.

```         
-- create temp example data
DROP TABLE IF EXISTS temp_sales_data;
CREATE TEMP TABLE temp_sales_data (
    product_name VARCHAR,
    sale_date DATE,
    purchase_count INT
);

INSERT INTO temp_sales_data (product_name, sale_date, purchase_count)
VALUES ('Apple', '2023-01-01', 10),
       ('Apple', '2023-01-02', 12),
       ('Orange', '2023-01-01', 8),
       ('Orange', '2023-01-02', 15);

-- pivoting data to wide format with CASE WHEN
SELECT
    sale_date,
    SUM(CASE WHEN product_name = 'Apple' THEN purchase_count ELSE 0 END) AS apple_sales,
    SUM(CASE WHEN product_name = 'Orange' THEN purchase_count ELSE 0 END) AS orange_sales
FROM temp_sales_data
GROUP BY 1
```

Crosstab to pivot data

```         
-- crosstab to pivot data
-- manually specify values for row values to turn into columns (daily dates below)
CREATE EXTENSION IF NOT EXISTS tablefunc;

SELECT *
FROM crosstab(
    'SELECT product_name, sale_date, purchase_count
     FROM temp_sales_data
     ORDER BY 1, 2',
    'SELECT DISTINCT sale_date FROM temp_sales_data ORDER BY 1'
) AS result(product_name VARCHAR, "2023-01-01" INT, "2023-01-02" INT);

-- create temp wide example data
DROP TABLE IF EXISTS temp_wide_sales_data;
CREATE TEMP TABLE temp_wide_sales_data (
    product_name VARCHAR,
    "2023-01-01" INT,
    "2023-01-02" INT
);

INSERT INTO temp_wide_sales_data (product_name, "2023-01-01", "2023-01-02")
VALUES ('Apple', 10, 12),
       ('Orange', 8, 15);

SELECT 
    product_name, 
    '2023-01-01'::date AS sale_date, 
    "2023-01-01" AS purchase_count
FROM temp_wide_sales_data

UNION ALL

SELECT 
    product_name, 
    '2023-01-02'::date AS sale_date, 
    "2023-01-02" AS purchase_count
FROM temp_wide_sales_data;
```

#### Generate Series

-   used to generate a series of values, from a start value to an end value with a step interval
-   useful for generating test/practice Postgres SQL datasets
-   **`start`**: the beginning value of the series
-   **`stop`**: the ending value of the series
-   **`step`**: the interval between each value in the series. If this is omitted, the default step is 1

```         
-- generate a series of dates for each day in January 2023
SELECT 
    date_ts::DATE as day_in_jan_2023 
FROM generate_series(
  '2023-01-01'::DATE, 
  '2023-01-31'::DATE, 
  interval '1 day'
) AS date_ts;

-- 100 ids from 1 to 100
SELECT id FROM generate_series(1, 100, 1) AS id;
```

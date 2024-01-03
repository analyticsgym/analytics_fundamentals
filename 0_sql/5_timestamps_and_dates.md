#### EXTRACT
- extract parts of a date/time value

```
WITH ts_example AS (
	SELECT 
		TO_TIMESTAMP('2021-03-13 09:30:03', 'YYYY-MM-DD HH:MI:SS') AS timestamp_1
)

SELECT
	EXTRACT(YEAR FROM timestamp_1) AS year,
	EXTRACT(QUARTER FROM timestamp_1) AS quarter,
	EXTRACT(MONTH FROM timestamp_1) AS month,
	EXTRACT(DOY FROM timestamp_1) AS day_of_year,
	EXTRACT(DAY FROM timestamp_1) AS day_of_month,
	EXTRACT(DOW FROM timestamp_1) AS day_of_week,
	EXTRACT(HOUR FROM timestamp_1) AS ts_hour,
	EXTRACT(MINUTE FROM timestamp_1) AS ts_minute,
	EXTRACT(SECOND FROM timestamp_1) AS ts_second
FROM ts_example
```

#### EXTRACT vs DATE_PART
- slight differences in DATE_PART vs EXTRACT; often used interchangeably
- some nuance: DATE_PART can grab fractional values with decimal points vs EXTRACT rounds down to nearest integer

```
WITH ts_example_2 AS (
	SELECT 
		TO_TIMESTAMP('2022-07-13 11:20:18:06', 'YYYY-MM-DD HH:MI:SS::MS') AS timestamp_2
)

SELECT
    DATE_PART('year', timestamp_2) AS dp_year,
    DATE_PART('quarter', timestamp_2) AS dp_quarter,
    DATE_PART('month', timestamp_2) AS dp_month,
    DATE_PART('day', timestamp_2) AS dp_day,
    DATE_PART('hour', timestamp_2) AS dp_hour,
    DATE_PART('minute', timestamp_2) AS dp_minute,
    -- fractional seconds example 
    DATE_PART('seconds', timestamp_2) AS dp_second
FROM ts_example_2
```

#### DATE_TRUNC
- similar to FLOOR function but for a date/time and specific unit of time (year, quarter, month, day, hour, minute, etc)
- commonly used to aggregate by date dimension (i.e. sales by month/quarter)

```
WITH ts_example_3 AS (
	SELECT 
		TO_TIMESTAMP('2022-05-22 07:30:03', 'YYYY-MM-DD HH:MI:SS') AS timestamp_3
)

SELECT
	*,
	DATE_TRUNC('quarter', timestamp_3)::DATE AS quarter,
	DATE_TRUNC('month', timestamp_3)::DATE AS month,
	-- simpler syntax for day
	timestamp_3::DATE AS day,
	DATE_TRUNC('hour', timestamp_3) AS hour
FROM ts_example_3
```

#### Date/Timestamp differences with AGE
- used to calculate differences between two given dates/timestamps or one given date/timestamp and current date/timestamp
- AGE function returns an interval data type
- works similar to DATEDIFF in other databases


```
-- generate timestamps and age interval
WITH age_example AS (
	-- sub query workaround (PostgreSQL has limitations on referencingcolumns previously specified in a query)
	SELECT
	  *,
	  AGE(purchased_at, home_page_first_visit) AS age_interval
	FROM (
	    SELECT 
		    TO_TIMESTAMP('2022-05-22 07:30:03', 'YYYY-MM-DD HH:MI:SS') AS home_page_first_visit,
		    TO_TIMESTAMP('2022-05-22 08:01:12', 'YYYY-MM-DD HH:MI:SS') AS purchased_at
	) AS sub_q
)

-- extract age interval parts
-- 1/4 pick back up here (pull out parts of age interval then combine for difference calcs)
SELECT
  *
FROM age_example
```

#### Date/Timestamp differences using direct subtraction
- To be added

#### Timestamp math (drop this section)
- NEEDS IMPROVEMENT for PostgreSQL (i.e. AGE function, etc)
- approaches differ by database
- PostgreSQL examples
- less verbose options with Amazon Redshift, etc

```
-- TODO : add more examples
WITH ts_math_example AS (SELECT
	TO_TIMESTAMP('2022-05-22 07:30:03', 'YYYY-MM-DD HH:MI:SS') AS subscription_start,
	TO_TIMESTAMP('2022-05-22 07:30:03', 'YYYY-MM-DD HH:MI:SS')  + INTERVAL '1 year' AS subscription_end
)

SELECT
	subscription_start,
	subscription_end,
  EXTRACT(YEAR FROM AGE(subscription_end, subscription_start)) * 12 + 
    EXTRACT(MONTH FROM AGE(subscription_end, subscription_start)) AS months_difference
FROM ts_math_example
```

#### Day of quarter workaround
- day of function doesn't exist 

```
WITH ts_example AS (
	SELECT 
		TO_TIMESTAMP('2021-03-13 09:30:00', 'YYYY-MM-DD HH:MI:SS') AS timestamp_1
)

SELECT
	-- add 1 so count starts 1; default is 0
	EXTRACT(DAY FROM timestamp_1 - DATE_TRUNC('QUARTER', timestamp_1))+1 AS day_of_quarter
FROM ts_example
```

#### Timezones
- many databases default to using UTC (doesn't have daylight savings)
- confirm with data engineers the default database time zone to be safe
- note timezones with daylight savings will have 2 standard abbreviations (i.e. PST daylight savings not in effect and PDT daylight savings in effect)
- PostgreSQL does not have a built-in convert_timezone() function vs other database vendors do

```
-- PostgreSQL approach to convert from PDT to UTC
WITH example_ts AS (
	SELECT TIMESTAMP '2023-04-04 15:00:00' AT TIME ZONE 'PDT' AS timestamp_pdt
)

SELECT
	timestamp_pdt,
	timestamp_pdt AT TIME ZONE 'UTC' AS timestamp_utc
FROM example_ts
```

## Practical Use Cases

#### Calendar days vs 24-hour-windows difference

```
------------------
------------------
------------------
-- WIP / needs QA/iteration
------------------
------------------
------------------
WITH membership AS (
    SELECT *
    FROM (
        VALUES
            (1, '2022-04-05 10:00:00-07:00'::timestamp with time zone, '2022-04-05 12:00:00-07:00'::timestamp with time zone),
            (2, '2022-04-06 23:00:00-07:00'::timestamp with time zone, '2023-04-07 01:00:00-07:00'::timestamp with time zone),
            (3, '2022-04-07 10:00:00-07:00'::timestamp with time zone, '2022-04-08 10:00:00-07:00'::timestamp with time zone)
    ) AS membership(member_id, start_time, end_time)
)
SELECT
	*,
	end_time::date - start_time::date AS calendar_days_difference,
	extract(epoch from (end_time - start_time)) AS diff_seconds,
	extract(seconds from (end_time - start_time)) AS diff_seconds_2,
	extract(hours from (end_time - start_time)) AS diff_hours,
	extract(epoch from (end_time - start_time)) / (60 * 60 * 24) AS twenty_four_interval_difference
FROM membership;
```


#### Leap years
- output leap years using leap year rule filtering

```
WITH years AS (
  SELECT generate_series(1900, 3000) AS year
)
SELECT 
	year
FROM year
-- <<< leap year rule >>>
-- If a year is evenly divisible by 4, it is a leap year, except:
-- If the year is evenly divisible by 100, it is NOT a leap year, unless:
-- The year is also evenly divisible by 400, in which case it is a leap year.
WHERE (year % 4 = 0 AND year % 100 <> 0) OR (year % 400 = 0);
```

#### Round a date timestamp to the nearest calendar day
- conditional 

```
WITH sample_data AS (
	SELECT '2022-04-02 00:06:00'::timestamp AS original_timestamp
		UNION
	SELECT '2022-04-02 08:21:00'::timestamp AS original_timestamp
		UNION 
  SELECT '2022-04-02 16:30:00'::timestamp AS original_timestamp
)

SELECT
    original_timestamp,
    CASE
        WHEN EXTRACT(hour FROM original_timestamp) < 12 THEN
            date_trunc('day', original_timestamp)
        ELSE
            date_trunc('day', original_timestamp) + INTERVAL '1 day'
    END AS rounded_to_nearest_calendar_day
FROM sample_data;
```

#### Hard code date string to use as filter 
- useful when a query filters on a date in multiple locations
- update the date filter once vs multiple locations

```
WITH filter_date AS (
	-- cast string to date type
	SELECT '2023-03-12'::DATE as filter_var
)

SELECT
	filter_var
FROM filter_date
```

#### Find all date gaps in a series of dates
- left join to the complete date sequence and filter on NULLs in the table where we want to surface date gaps

```
DROP TABLE IF EXISTS temp_new_customers;
CREATE TEMP TABLE temp_new_customers AS

-- example reporting data on new customers by day 
SELECT 
  DATE_TRUNC('day', purchase_date) AS purchase_date,
  ROUND((RANDOM()::NUMERIC * 10),2) AS first_time_purchasers
FROM GENERATE_SERIES(
        '2022-01-01'::timestamp, 
        '2022-12-31'::timestamp, 
        '1 day'::interval
    ) AS purchase_date;

-- set seed to generate have same results on query reruns
SELECT SETSEED(0.1);

-- randomly drop rows to create intentional date gaps
DELETE FROM temp_new_customers
WHERE RANDOM() < 0.025;

-- generate date sequence without gaps
WITH complete_date_sequence AS (
SELECT 
  DATE_TRUNC('day', complete_date) AS sequence_date
FROM GENERATE_SERIES(
	  (SELECT MIN(purchase_date) FROM temp_new_customers),
	  (SELECT MAX(purchase_date) FROM temp_new_customers),
	  '1 day'::interval
	) AS complete_date
)

-- return missing dates
SELECT 
	c.sequence_date
FROM complete_date_sequence AS c
LEFT JOIN temp_new_customers AS t 
	ON t.purchase_date = c.sequence_date
WHERE t.purchase_date IS NULL
```

#### Fill in date gaps in a series of dates
- for the date gaps in the new customers table we get NULL values for `first_time_purchasers` and replace the NULLs with 0 so have a complete date sequence

```
-- using example data created above
-- date gaps filled and 0s used when gap exists
SELECT 
	c.sequence_date,
	COALESCE(t.first_time_purchasers, 0) AS first_time_purchasers_clean
FROM complete_date_sequence AS c
LEFT JOIN temp_new_customers AS t 
	ON t.purchase_date = c.sequence_date
ORDER BY first_time_purchasers_clean ASC
```

#### Filter dates
- remember BETWEEN clause is inclusive of beginning and end value

```
WITH monthly_sales (year_month, sales_amount) AS (
  VALUES
    ('2021-10', 20),
    ('2021-11', 30),
    ('2021-12', 40),
    ('2022-01', 90),
    ('2022-02', 120),
    ('2022-03', 200),
    ('2022-04', 300),
    ('2022-05', 400),
    ('2022-06', 650),
    ('2022-07', 400),
    ('2022-08', 450),
    ('2022-09', 500),
    ('2022-10', 550),
    ('2022-11', 600),
    ('2022-12', 650),
    ('2023-01', 700),
    ('2023-02', 750),
    ('2023-03', 800),
    ('2023-04', 850)
)

SELECT
	*
FROM monthly_sales
-- use BETWEEN to grab 2022 months
WHERE TO_DATE(year_month, 'YYYY-MM') BETWEEN '2022-01-01'::DATE AND '2022-12-01'::DATE
```


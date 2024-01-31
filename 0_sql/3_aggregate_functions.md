# Aggregate Functions

#### Aggregate functions overview

-   return a single result output from set of input values
-   common calculation based aggregate functions: COUNT, SUM, AVG, MAX, MIN , etc
-   other types of aggregate functions: STRING_AGG, ARRAY_AGG, etc

#### Aggregates/metrics with NULL values

-   by default, most aggregate functions ignore NULL values
-   COALESCE can be used set NULLs to a default value to be included in calcs
-   important to consider if NULLs should or should not be included in calcs when SQL functions

```sql         
WITH pet_goats(name, pet_goats) AS (
  VALUES
    ('becky', 4),
    ('jill', 2),
    ('becky', NULL),
    ('tom', 1)
)

-- as expected, average shifts based on how NULLs are handled
SELECT
    -- ignores NULL values by default
	AVG(pet_goats::FLOAT) AS avg_pet_goats_per_pet_person,
	-- NULL values set to 0
	AVG(COALESCE(pet_goats, 0)::FLOAT) AS avg_pet_goats_per_person_including_of_non_owners
FROM pet_goats
```

#### COUNT
-   Next step 2/1 bookmark
-   Subtle differences of how COUNT functions can be applied based on use case.
-   COUNT(\*) counts number of rows.
-   COUNT(<column_name>) count of non-NULL column values.
-   COUNT(DISTINCT <column_name>) count unique of non-NULL column values.

```         
SELECT
    COUNT(*) AS cte_row_count,
    COUNT(pet_goats) AS number_of_pet_goat_owners,
    COUNT(*) - COUNT(pet_goats) AS number_of_non_pet_goat_owners,
    COUNT(DISTINCT name) AS number_of_unique_pet_goat_owner_names
-- example CTE generated above
FROM pet_goats
```

#### Median and Percentiles

-   PERCENTILE_DIST: returns an existing data point at or exceeding the percentile threshold (does not use interpolation)
-   PERCENTILE_CONT: returns a percentile value with interpolation if needed
-   percentiles represent the proportion of values in a distribution that are less than the percentile value
-   interpolation vs extrapolation: interpolation estimates a value between two known values vs extrapolation estimates a value beyond known values
-   continuous vs discrete percentile: continuous percentile is the proportion of values less than the percentile value vs discrete percentile is the proportion of values less than or equal to the percentile value
-   median = 50th percentile = 0.5 continuous percentile

``` sql
WITH student_scores(student_id, test_score) AS (
            VALUES
            (1, 85),
            (2, 78),
            (3, 92),
            (4, 88),
            (5, 74),
            (6, 81),
            (7, 67),
            (8, 95),
            (9, 89),
            (10, 72),
            (11, 90),
            (12, 77),
            (13, 83),
            (14, 65),
            (15, 80)
)

SELECT 
    DISTINCT
    -- continuous percentile
    PERCENTILE_CONT(0.05) WITHIN GROUP (ORDER BY test_score) AS p05_score,
    PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY test_score) AS p25_score,
    PERCENTILE_CONT(0.50) WITHIN GROUP (ORDER BY test_score) AS median_score,
    PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY test_score) AS p75_score,
    PERCENTILE_CONT(0.95) WITHIN GROUP (ORDER BY test_score) AS p95_score,
    -- discrete percentile
    PERCENTILE_DISC(0.05) WITHIN GROUP (ORDER BY test_score) AS discrete_05_score,
    PERCENTILE_DISC(0.25) WITHIN GROUP (ORDER BY test_score) AS discrete_25_score,
    PERCENTILE_DISC(0.50) WITHIN GROUP (ORDER BY test_score) AS discrete_median_score,
    PERCENTILE_DISC(0.75) WITHIN GROUP (ORDER BY test_score) AS discrete_75_score,
    PERCENTILE_DISC(0.95) WITHIN GROUP (ORDER BY test_score) AS discrete_95_score
FROM student_scores
```

#### Variance and Standard Deviation

-   VAR_POP and STDDEV_POP used for entire population datasets.
-   VAR_SAMP and STDDEV_SAMP used for samples (adjustments made for sample size vs population metrics).
-   In general, for large sample sizes/data sets, population vs sample variance/standard deviation will be similar.
-   Standard deviation is a helpful metric for assessing the variability/dispersion of a dataset alongside measures of centrality (i.e. averages, etc).

```         
WITH batting_sample AS (
    SELECT 
        UNNEST(ARRAY[1,2,3,4,5,6,7,8,9,10,11,12]) AS player,
        UNNEST(ARRAY[0.287, 0.312, 0.254, 0.326, 0.268, 0.291, 
                     0.299, 0.276, 0.305, 0.282, 0.320, 0.294]) AS batting_average
)

SELECT
    AVG(batting_average::FLOAT) AS average_batting_average,
    VAR_SAMP(batting_average::FLOAT) AS sample_variance,
    STDDEV_SAMP(batting_average::FLOAT) AS sample_standard_deviation
FROM batting_sample 
```

#### Binary Flags with MAX

-   Common approach for creating product usage flags.

```         
-- update witb cleaner code
WITH installs AS (
  SELECT * FROM (
      VALUES
        (1, '2023-01-01', 'iOS'),
        (1, '2023-01-02', 'Android'),
        (1, '2023-01-02', 'Apple TV'),
        (2, '2023-01-03', 'iOS'),
        (3, '2023-01-04', 'Android'),
        (4, '2023-01-05', 'iOS'),
        (4, '2023-01-05', 'Fire TV'),
        (4, '2023-01-05', 'Android TV')
  ) AS t(user_id, install_date, platform)
)

SELECT
  user_id,
  MAX(CASE WHEN platform = 'iOS' THEN 1 ELSE 0 END) AS flag_ios_installed,
  MAX(CASE WHEN platform = 'Android' THEN 1 ELSE 0 END) AS flag_android_installed,
  MAX(CASE WHEN platform = 'Apple TV' THEN 1 ELSE 0 END) AS flag_apple_tv_installed,
  MAX(CASE WHEN platform = 'Android TV' THEN 1 ELSE 0 END) AS flag_apple_tv_installed,
  MAX(CASE WHEN platform = 'Fire TV' THEN 1 ELSE 0 END) AS flag_fire_tv_installed
FROM installs
GROUP BY user_id;
```

#### Misc notes

-   creating bins
-   

#### Conditional aggregate functions

### Aggregate functions on Numerics, Integers, Floats

-   integer data types for counting or summing small whole numbers
-   floating-point data types for calculations involving decimal numbers
-   numeric data types for precise calculations involving large or small numbers

```         
Include examples
```

#### AVG on INT vs FLOAT

-   by default Postgres SQL will round integer average to nearest whole number
-   cast to FLOAT to ensure decimal places are returned
-   note: some query tools will not round average to nearest whole number

```         

#### Flagging outliers

#### GROUPING SETS
- used to output agggregation result for different granularities within the same group by clause (i.e. agg sales for group 1 overall, agg sales for group 1 and 2, etc)
- could also use UNIONs to row bind results of varying granularity together with matching column names (more verbose code needed)
- not available in Amazon Redshift (use UNION ALL approach instead)
```

WITH zip_sales(state, city, total_sales, fake_zipcode) AS ( VALUES ('NY', 'New York', 50000.00, 12345), ('NY', 'Buffalo', 20000.00, 45678), ('CA', 'Los Angeles', 75000.00, 54321), ('CA', 'San Francisco', 60000.00, 56789), ('TX', 'Houston', 45000.00, 11222), ('TX', 'Houston', 45000.00, 11223), ('TX', 'Dallas', 40000.00, 33444) )

SELECT fake_zipcode, city, state, SUM(total_sales) AS sales_agg FROM zip_sales GROUP BY GROUPING SETS( (fake_zipcode), (city, state), (state) )

```         

UNION ALL when GROUPING SETS function not available
```

-- using CTE data from above code SELECT 'fake_zipcode' AS grain_label, fake_zipcode::VARCHAR, NULL AS city, NULL AS state, total_sales AS sales_agg -- group by aggregation not needed here given data is at the zip level FROM zip_sales

UNION ALL

SELECT 'city and state' AS grain_label, NULL AS fake_zipcode, city, state, SUM(total_sales) AS sales_agg FROM zip_sales GROUP BY 1,2,3,4

UNION ALL

SELECT 'state' AS grain_label, NULL AS fake_zipcode, NULL AS city, state, SUM(total_sales) AS sales_agg FROM zip_sales GROUP BY 1,2,3,4

```         


DROP TABLE IF EXISTS net_worth_temp_table;
CREATE TEMP TABLE net_worth_temp_table (
  name_id SERIAL PRIMARY KEY,
  net_worth_millions INTEGER
);
INSERT INTO net_worth_temp_table (net_worth_millions) VALUES (10), (20), (1), (6), (11);
```

#### Aggregate functions with filters

-   Needs update the below code is a mess

```         
-- CREATE TEMP TABLE orders_test ( -- id SERIAL PRIMARY KEY, -- customer TEXT NOT NULL, -- total NUMERIC(10, 2) NOT NULL, -- date DATE NOT NULL -- );

-- INSERT INTO orders_test (customer, total, date) -- VALUES -- ('Alice', 10.00, '2022-01-01'), -- ('Bob', 20.00, '2022-01-01'), -- ('Alice', 15.00, '2022-01-02'), -- ('Charlie', 25.00, '2022-01-02'), -- ('Alice', 30.00, '2022-01-03');

-- SELECT customer, AVG(total) FILTER (WHERE date \>= '2022-01-02') -- FROM orders_test -- GROUP BY customer;

SELECT customer, SUM(total) FILTER (WHERE date = '2022-01-01') AS jan_1_sales, SUM(total) FILTER (WHERE date = '2022-01-02') AS jan_2_sales, SUM(total) FILTER (WHERE date = '2022-01-03') AS jan_3_sales FROM orders_test GROUP BY customer;
```

#### Sales growth vs baseline year

-   one of several approaches to achieve desired end result

```         
-- example output from a query to aggregate sales by year
WITH sales AS (
    SELECT 
        UNNEST(ARRAY[23456, 43567, 65812, 79234, 567394]) AS annual_sales,
        UNNEST(ARRAY[1999, 2000, 2001, 2002, 2003]) AS year
)

, baseline_year AS (
SELECT
    annual_sales,
    year
FROM sales
WHERE year = (SELECT MIN(year) FROM sales)
)

SELECT 
    s.*,
    b.annual_sales AS baseline_sales,
    CASE
        WHEN b.annual_sales IS NULL THEN 0
        ELSE 100 * (s.annual_sales - b.annual_sales)::FLOAT /(b.annual_sales)
    END AS percent_change_vs_baseline
FROM sales AS s
INNER JOIN baseline_year AS b
    ON 1=1
```

#### Derive z-score

-   z-scores used to asses how far away data points are from the mean in terms of standard deviations from the mean
-   i.e. z-score of 0: data point is equal to the mean, z-score of 1: data point 1 standard deviation above the mean, z-score of -1: data point 1 standard deviation below the mean

```         
WITH batting_sample AS (
    SELECT 
        UNNEST(ARRAY[1,2,3,4,5,6,7,8,9,10,11,12]) AS player,
        UNNEST(ARRAY[0.287, 0.312, 0.254, 0.326, 0.268, 0.291, 
                     0.299, 0.276, 0.305, 0.282, 0.320, 0.294]) AS batting_average
)

, sample_stats AS (
SELECT
    AVG(batting_average::FLOAT) AS average_batting_average,
    VAR_SAMP(batting_average::FLOAT) AS sample_variance,
    STDDEV_SAMP(batting_average::FLOAT) AS sample_standard_deviation
FROM batting_sample 
)

SELECT
    bs.player,
    bs.batting_average,
    s.average_batting_average AS sample_average_batting_average,
    (bs.batting_average - s.average_batting_average) / 
        s.sample_standard_deviation::FLOAT AS batting_average_zscore
FROM batting_sample AS bs
INNER JOIN sample_stats AS s
    ON 1=1
ORDER BY bs.batting_average DESC
```

#### Derive elements used to draw a boxplot

-   lower whisker edge: 25th percentile - 1.5\*IQR
-   lower box edge: 25th percentile
-   median box line: 50th percenitle
-   upper box edge: 75th percentile
-   upper whisker edge: 75th percentile + 1.5\*IQR
-   note: points outside lower and upper whisker edge tend to plotted as points and represent outliers on the boxplot

```         
WITH test_scores(score) AS (
  VALUES 
  (41), (70), (81), (32), (75), 
  (51), (62), (83), (84), (79),
  (75), (76), (76), (78), (80),
  (79), (74), (60), (24), (19),
  (59), (67), (83), (98), (99)
)

, box_plot_values AS (
  SELECT 
    -- lower whisker edge = min data value if derived lower whisker edge exceeds min value in data
      GREATEST(
      MIN(score),
      PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY score) - 
      1.5 * (PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY score) - 
             PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY score))
    ) AS lower_whisker_edge,
    
    PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY score) AS lower_box_edge,
    
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY score) AS median_box_line,

    PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY score) AS upper_box_edge,

    -- upper whisker edge = max data value if derived upper whisker edge exceeds max value in data
      LEAST(
      MAX(score),
      PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY score) + 
      1.5 * (PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY score) - 
             PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY score))
    ) AS upper_whisker_edge
  FROM test_scores
)

-- if lower outliers exist, generate text string of outliers
, boxplot_low_outliers AS (
SELECT
    COALESCE(STRING_AGG(score::VARCHAR, ', ' ORDER BY score ASC), 'no lower outliers') AS lower_outliers
FROM test_scores
WHERE score < (SELECT lower_whisker_edge FROM box_plot_values)
)

-- if upper outliers exist, generate text string of outliers
, boxplot_up_outliers AS (
SELECT
    COALESCE(STRING_AGG(score::VARCHAR, ', ' ORDER BY score), 'no upper outliers') AS upper_outliers
FROM test_scores
WHERE score > (SELECT upper_whisker_edge FROM box_plot_values)
)

-- values used to generate a boxplot 
SELECT
    l.lower_outliers,
    b.lower_whisker_edge,
    b.lower_box_edge,
    b.median_box_line,
    b.upper_box_edge,
    b.upper_whisker_edge,
    u.upper_outliers
FROM box_plot_values AS b
INNER JOIN boxplot_up_outliers AS u
    ON 1=1
INNER JOIN boxplot_low_outliers AS l
    ON 1=1
```

### Pre vs Post Analysis

-   sql to generate simple pre vs post overall conversion rate summary data

```         
WITH conversion_data(date, site_visitors, conversions) AS (
    VALUES
        ('2023-05-01'::DATE, 100::INTEGER, 20::INTEGER),
        ('2023-05-02', 110, 19),
        ('2023-05-03', 120, 22),
        ('2023-05-04', 105, 21),
        ('2023-05-05', 115, 20),
        ('2023-05-06', 125, 24),
        ('2023-05-07', 130, 26),
        ('2023-05-08', 135, 28),
        ('2023-05-09', 140, 30),
        ('2023-05-10', 120, 24),
        ('2023-05-11', 155, 31),
        ('2023-05-12', 130, 26),
        ('2023-05-13', 140, 28),
        ('2023-05-14', 160, 32), 
        ('2023-05-15', 165, 37),
        ('2023-05-16', 180, 42),
        ('2023-05-17', 190, 47),
        ('2023-05-18', 200, 55),
        ('2023-05-19', 210, 60),
        ('2023-05-20', 220, 66),
        ('2023-05-21', 190, 53),
        ('2023-05-22', 195, 58),
        ('2023-05-23', 200, 63),
        ('2023-05-24', 205, 68),
        ('2023-05-25', 210, 73),
        ('2023-05-26', 215, 78),
        ('2023-05-27', 220, 83),
        ('2023-05-28', 225, 88)
)

SELECT
    CASE
        WHEN date BETWEEN '2023-05-01'::DATE AND '2023-05-14'::DATE
        THEN 'pre_period'
        WHEN date BETWEEN '2023-05-15'::DATE AND '2023-05-28'::DATE
        THEN 'post_period'
        ELSE 'error'
    END AS measurement_window,
    MIN(date) AS window_start_date, 
    MAX(date) AS window_end_date, 
    COUNT(DISTINCT date) AS measurement_period_days, 
    SUM(site_visitors) AS total_visitors,
    SUM(conversions) AS total_conversions,
    (SUM(conversions)::FLOAT / SUM(site_visitors))*100 AS window_conversion_rate
FROM conversion_data
GROUP BY 1
ORDER BY window_start_date ASC
```

## TODO / Revisit

#### Revisit!!!

- revisit; how is this practical? When would you use this?  
- above examples for PERCENTILE_CONT & PERCENTILE_DISC highlight wide format output

```         
WITH student_scores(student_id, test_score) AS (
            VALUES
            (1, 85),
            (2, 78),
            (3, 92),
            (4, 88),
            (5, 74),
            (6, 81),
            (7, 67),
            (8, 95),
            (9, 89),
            (10, 72),
            (11, 90),
            (12, 77),
            (13, 83),
            (14, 65),
            (15, 80)
)

, percentile_names(name, value) AS (
    VALUES
    ('p05', 0.05),
    ('p25', 0.25),
    ('median', 0.50),
    ('p75', 0.75),
    ('p95', 0.95)
)

, percentile_type(pct_type) AS (
    VALUES
    ('continuous'), 
    ('discrete')
)

, percentile_type_and_name AS (
    SELECT
        pn.*,
        pt.*
    FROM percentile_names AS pn
    CROSS JOIN percentile_type AS pt
)

SELECT
    ptn.pct_type AS percentile_type,
    ptn.name AS percentile_name,
    CASE
        WHEN ptn.pct_type = 'continuous' 
                THEN PERCENTILE_CONT(ptn.value) WITHIN GROUP (ORDER BY ss.test_score)
        WHEN ptn.pct_type = 'discrete'
        THEN PERCENTILE_DISC(ptn.value) WITHIN GROUP (ORDER BY ss.test_score)
    END AS percentile_value
FROM student_scores AS ss
CROSS JOIN percentile_type_and_name AS ptn
GROUP BY ptn.pct_type, ptn.name, ptn.value
ORDER BY ptn.pct_type, ptn.value;
```

#### Capping values

-   useful when dataset has long tails and there's a desire to reduce the influence of extreme outlier values on metrics
-   example logic: if a value is below 10th percentile cap the value at the 10th percentile; if a value is above 90th percentile cap the value at the 90th percentile

```         
WITH example_data(result) AS (
   VALUES 
    (1), (2), (3), (4), (5), (6), (7), (8), (9), (10), 
    (11), (12), (13), (14), (15), (16), (17), (18), (19), (20)
)

, percentiles AS (
SELECT 
    percentile_cont(0.10) WITHIN GROUP (ORDER BY result) AS perc_10,
    percentile_cont(0.90) WITHIN GROUP (ORDER BY result) AS perc_90
FROM example_data
)

SELECT 
  result, 
  CASE 
    WHEN result < perc_10 THEN perc_10
    WHEN result > perc_90 THEN perc_90
    ELSE result
  END as capped_result
FROM example_data AS ed
INNER JOIN percentiles AS p
    ON 1=1
```

#### Hypothetical-Set Aggregate Functions
- look similar to window functions but are not window functions
- calculating the result of a hypothetical value as if it were part of the set of likes
- TODO: explore further; included in other DBs, explain difference with window functions

```sql
WITH social_media_posts(post_id, user_id, likes, post_date) AS (
  VALUES 
  (1, 101, 120, '2024-01-01'),
  (2, 102, 150, '2024-01-02'),
  (3, 101, 200, '2024-01-02'),
  (4, 103, 180, '2024-01-03'),
  (5, 102, 140, '2024-01-04'),
  (6, 104, 160, '2024-01-04'),
  (7, 101, 190, '2024-01-04'),
  (8, 105, 110, '2024-01-05'),
  (9, 104, 130, '2024-01-05'),
  (10, 103, 175, '2024-01-06'),
  (11, 101, 165, '2024-01-06'),
  (12, 102, 155, '2024-01-07'),
  (13, 103, 145, '2024-01-07'),
  (14, 104, 135, '2024-01-08'),
  (15, 105, 125, '2024-01-08')
)
SELECT 
	rank(500) WITHIN GROUP (ORDER BY likes DESC) AS test1,
	rank(105) WITHIN GROUP (ORDER BY likes DESC) AS test2
FROM social_media_posts;
```



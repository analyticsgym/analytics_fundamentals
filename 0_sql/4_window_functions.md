#### Window functions basics
- used to apply functions across a window or result set of data
- window function with a partition applies the function independently to each partition/cut

```
-- window function pattern
SELECT
	<desired_columns>,
	WINDOW_FUNCTION() OVER(
		PARTITION BY <used to specify if the function should be applied to cuts of the data>
		ORDER BY <used to specify ordering of data (i.e. ranking approaches)>
		ROWS <used to specify which rows the within the partition to apply the function>
	) AS window_metric_x
FROM <table_name>
```

#### Types of window frames
- window frame clause used to describe which records to process in relation to the current row and partition given
- without a partition or window frame specified, the window function will process the entire dataset for the given function in relation to the current row
- ROWS frame: define by fixed number of rows relative to the current row (below frame boundary keywords can be used to specify number of rows)
- RANGE frame: include records within boundary in relation to current row (below keywords can be used like ROWS)
- GROUPS frame: defined by a fixed number of peer groups relative to the current row's peer group (not included as part of all DB vendors using SQL)

#### Window frame boundaries
- Default window frame boundary **with** ORDER BY clause: "UNBOUNDED PRECEDING" to "CURRENT ROW"
- Default window frame boundary **without** ORDER BY clause used: entire window partition (e.g. "UNBOUNDED PRECEDING" to "UNBOUNDED FOLLOWING")
- UNBOUNDED PRECEDING: includes all rows before the current row
- UNBOUNDED FOLLOWING: includes all rows after the current row
- CURRENT ROW: includes only the current row 
- N PRECEEDING: include N number of rows before current row
- N FOLLOWING: include N number of rows after current row

#### Frame exclusions
- PostgreSQL also supports the frame exclusion keywords which can be used to exclude rows from the window frame operation
- not available across all DB vendors
- examples include: EXCLUDE CURRENT ROW, EXCLUDE GROUP, EXCLUDE TIES

#### Aggregation functions as window functions
- COUNT, SUM, AVG, etc

```
-- DROP TABLE IF EXISTS friends_example_table;
CREATE TEMP TABLE friends_example_table (
  name TEXT NOT NULL,
  gender CHAR(1) NOT NULL,
  street_address TEXT,
  attended_four_year_college BOOLEAN NOT NULL
);

INSERT INTO friends_example_table (name, gender, street_address, attended_four_year_college)
VALUES
  ('John Doe', 'M', '123 Main St', true),
  ('Jane Smith', 'F', NULL, false),
  ('Rain Smith', 'M', NULL, false),
  ('Sue Johnson', 'F', '456 Elm St', true),
  ('Bob Johnson', 'M', '456 Elm St', true),
  ('Sara Lee', 'F', NULL, true),
  ('Mark Brown', 'M', '789 Maple Ave', false);

SELECT
	*,
	COUNT(street_address) OVER(
		PARTITION BY gender
	) AS street_address_count_by_gender_group
FROM friends_example_table
```

#### ROW_NUMBER() 
- assigns a unique integer to each row in the result set

```
DROP TABLE IF EXISTS temp_sales_data;
CREATE TEMP TABLE temp_sales_data (
    id SERIAL PRIMARY KEY,
    sales_rep VARCHAR(255),
    sales_amount NUMERIC(10, 2)
);

INSERT INTO temp_sales_data (sales_rep, sales_amount)
VALUES ('Alice', 25000),
       ('Bob', 30000),
       ('Carol', 45000),
       ('David', 60000),
       ('Eve', 55000),
       ('Frank', 70000),
       ('Grace', 80000),
       ('Hill', 80000),
       ('Iris', 21000),
       ('Jan', 90000);

SELECT
    sales_rep,
    sales_amount,
    -- if sales amount tie then revert to alphabetical name  
    ROW_NUMBER() OVER(ORDER BY sales_amount DESC, sales_rep) AS sales_rank
FROM temp_sales_data
ORDER BY sales_amount DESC, sales_rep;
```

#### RANK() 
- assigns a rank to each row within a result set, with ties receiving the same rank
- note that subsequent ranks get skipped when ties occur

```
SELECT
    sales_rep,
    sales_amount,
    -- allow for same rank if ties
    RANK() OVER(ORDER BY sales_amount DESC) AS sales_rank
FROM temp_sales_data
ORDER BY sales_amount DESC, sales_rep;
```

#### DENSE_RANK() 
- assigns a rank to each row within a result set, with ties receiving the same rank, but without gaps between ranks

```
SELECT
    sales_rep,
    sales_amount,
    -- allow for same rank if ties
    RANK() OVER(ORDER BY sales_amount DESC) AS sales_rank,
    DENSE_RANK() OVER(ORDER BY sales_amount DESC) AS sales_dense_rank
FROM temp_sales_data
ORDER BY sales_amount DESC, sales_rep;
```

#### PERCENT_RANK() 
- returns a percentage value between 0 and 1, representing the position of the current row relative to the other rows in the result set, ordered by a specific column or columns

```
SELECT
    sales_rep,
    sales_amount,
    -- allow for same rank if ties
    RANK() OVER(ORDER BY sales_amount DESC) AS sales_rank,
    DENSE_RANK() OVER(ORDER BY sales_amount DESC) AS sales_dense_rank,
    PERCENT_RANK() OVER(ORDER BY sales_amount DESC) AS pct_rank,
		PERCENT_RANK() OVER(ORDER BY sales_amount DESC) <= 0.2 AS flag_top_20_pct_seller
FROM temp_sales_data
ORDER BY sales_amount DESC, sales_rep;
```

#### CUME_DIST()
- calculate the relative position or percentile rank of a value within a dataset
- returns value between 0 and 1
- count of rows with values <= ith row value / count of rows in the window or partition
- this is not the same as cumulative percent total (see practical examples section for using window functions to derive cumulative percent total)

```
WITH salary AS (
	SELECT unnest(ARRAY[50000, 55000, 60000, 65000, 70000, 
											75000, 80000, 85000, 90000, 950000]) AS salary_dollars
)

SELECT
	salary_dollars,
	CUME_DIST() OVER(ORDER BY salary_dollars) AS cumulative_distribution_value
FROM salary
```

#### NTILE(n) 
- divides the result set into n groups of roughly equal size and assigns a group number to each row
- pay close attention to how ordering is done within the partition (i.e. depending on use case, should largest values be in first or last NTILE?)

```
WITH fake_book_sales(book_id, book_title, copies_sold) AS (
	        VALUES
            (101, 'The Catcher in the Rye', 800),
            (102, 'To Kill a Mockingbird', 1200),
            (103, 'The Great Gatsby', 600),
            (104, '1984', 900),
            (105, 'Pride and Prejudice', 500),
            (106, 'Animal Farm', 1000),
            (107, 'Brave New World', 750),
            (108, 'Moby-Dick', 450),
            (109, 'Crime and Punishment', 550),
            (110, 'Lord of the Flies', 700),
            (111, 'A Raisin in the Sun', 1700),
            (112, 'The Grapes of Wrath', 1050)
)

, perf AS (
SELECT
  *,
  NTILE(4) OVER(ORDER BY copies_sold DESC) AS performance_quartile
FROM fake_book_sales
)

SELECT
	book_id, 
	book_title, 
	copies_sold,
	-- tiers based on ntile ranking quartiles
	'tier_' || performance_quartile AS performance_tier
FROM perf
```

#### LAG() | LEAD()
- LAG: returns the value of the expression evaluated at the row that is offset rows before the current row
- LEAD: returns the value of the expression evaluated at the row that is offset rows after the current row

```
WITH example_watch_history(user_id, user_name, show_id, show_title, first_watched_date) AS (
    VALUES
        -- user 1 example 
	      (1, 'Alice', 101, 'Stranger Things', '2023-04-01'),
        (1, 'Alice', 102, 'The Crown', '2023-04-02'),
        (1, 'Alice', 103, 'The Witcher', '2023-04-03'),
        (1, 'Alice', 104, 'Money Heist', '2023-04-04'),
        -- user 2 example
        (2, 'Bob', 105, 'Breaking Bad', '2023-04-01'),
        (2, 'Bob', 106, 'The Queen''s Gambit', '2023-04-02'),
        (2, 'Bob', 107, 'Narcos', '2023-04-03'),
        (2, 'Bob', 108, 'Dark', '2023-04-04'),
        -- user 3 example
        (3, 'Charlie', 109, 'Ozark', '2023-04-01'),
        (3, 'Charlie', 110, 'The Mandalorian', '2023-04-02'),
        (3, 'Charlie', 111, 'The Boys', '2023-04-03'),
        (3, 'Charlie', 112, 'Fleabag', '2023-04-04'),
        -- user 4 example
        (4, 'Dave', 113, 'Killing Eve', '2023-04-01'),
        (4, 'Dave', 114, 'The Umbrella Academy', '2023-04-02'),
        (4, 'Dave', 115, 'Mindhunter', '2023-04-03'),
        (4, 'Dave', 116, 'The Haunting of Bly Manor', '2023-04-04'),
        -- user 5 example
        (5, 'Eve', 117, 'The Witcher', '2023-04-01'),
        (5, 'Eve', 118, 'Bridgerton', '2023-04-02'),
        (5, 'Eve', 119, 'The Expanse', '2023-04-03'),
        (5, 'Eve', 120, 'Schitt''s Creek', '2023-04-04')
)

SELECT
	*,
	-- assumes show titles are unique in the example dataset
	LAG(show_title) OVER (
		PARTITION BY user_id 
		ORDER BY first_watched_date
	) AS prev_show_watched,
  LEAD(show_title) OVER (
		PARTITION BY user_id 
		ORDER BY first_watched_date
	) AS next_show_watched
FROM example_watch_history
```

#### FIRST_VALUE() | LAST_VALUE(expr) | NTH_VALUE()
- FIRST_VALUE: returns the value of the expression evaluated at the first row of the window frame
- LAST_VALUE: returns the value of the expression evaluated at the last row of the window frame
- NTH_VALUE: returns the value of the expression evaluated at the nth row of the window frame

```
SELECT
	*,
	-- assumes show titles are unique in the example dataset
	LAG(show_title) OVER (
		PARTITION BY user_id 
		ORDER BY first_watched_date
	) AS prev_show_watched,
  LEAD(show_title) OVER (
		PARTITION BY user_id 
		ORDER BY first_watched_date
	) AS next_show_watched,
	FIRST_VALUE(show_title) OVER(
		PARTITION BY user_id 
		ORDER BY first_watched_date	
		ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
	) AS first_show_watched,
	NTH_VALUE(show_title, 2) OVER(
		PARTITION BY user_id 
		ORDER BY first_watched_date
		ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
	) AS second_show_watched,
	NTH_VALUE(show_title, 3) OVER(
		PARTITION BY user_id 
		ORDER BY first_watched_date
		ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
	) AS third_show_watched,
  LAST_VALUE(show_title) OVER(
		PARTITION BY user_id 
		ORDER BY first_watched_date
		ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
	) AS last_show_watched
-- CTE generated above
FROM example_watch_history
```

```
-- example 2
WITH user_movies_watched(user_id, movie_num, movie_name) AS (
    VALUES
    (1, 1, 'Movie1'),
    (1, 2, 'Movie2'),
    (1, 3, 'Movie3'),
    (1, 4, 'Movie4'),
    (1, 5, NULL), -- The user did not watch a 5th movie
    (2, 1, 'Movie5'),
    (2, 2, 'Movie6'),
    (2, 3, NULL), -- The user did not watch a 3rd movie
    (2, 4, NULL), -- The user did not watch a 4th movie
    (2, 5, NULL), -- The user did not watch a 5th movie
    (3, 1, 'Movie7'),
    (3, 2, 'Movie8'),
    (3, 3, 'Movie9'),
    (3, 4, 'Movie10'),
    (3, 5, 'Movie11')
)

SELECT
  DISTINCT
  user_id,
  FIRST_VALUE(movie_name) OVER (
	  PARTITION BY user_id 
	  ORDER BY movie_num
  ) AS first_movie,
  NTH_VALUE(movie_name, 2) OVER (
	  PARTITION BY user_id 
	  ORDER BY movie_num
  	  ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
  ) AS second_movie,
  NTH_VALUE(movie_name, 3) OVER (
	  PARTITION BY user_id 
	  ORDER BY movie_num
	  ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
  ) AS third_movie,
  NTH_VALUE(movie_name, 4) OVER (
	  PARTITION BY user_id 
	  ORDER BY movie_num
	  ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
  ) AS fourth_movie,
  NTH_VALUE(movie_name, 5) OVER (
	  PARTITION BY user_id 
	  ORDER BY movie_num
	  ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
  ) AS fifth_movie
FROM user_movies_watched
ORDER BY user_id
```

#### PERCENTILE_DIST() | PERCENTILE_CONT()
- PERCENTILE_DIST: returns an existing data point from the dataset without interpolation
- PERCENTILE_CONT: provides a more precise percentile value by allowing interpolation (i.e. 50th percentile doesn't land exactly on a number)
- percentiles represent the proportion of values in a distribution that are less than the percentile value

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

## Practical Use Cases

#### Percent of total 
- approach with PostgreSQL
- database vendors have slightly different approaches (i.e. Redshift has RATIO_TO_REPORT() which can be used for pct total calcs)

```
WITH example_data (item, amount) AS (
  VALUES
    ('Item A', 50),
    ('Item B', 100),
    ('Item C', 150),
    ('Item D', 200),
	  ('Item E', 13)
)
SELECT 
	item,
	amount, 
	ROUND((100.0 * amount / SUM(amount) OVER()),2) AS percent_total 
FROM example_data
ORDER BY percent_total  DESC;
```

#### Cumulative percent total using window functions
- rolling sum window function compared to total aggregation

```
WITH salary AS (
	SELECT unnest(ARRAY[50000, 55000, 60000, 65000, 70000, 
	                    75000, 80000, 85000, 90000, 950000]) AS salary_dollars
)

, total_salary_spend AS (
	SELECT SUM(salary_dollars) AS total FROM salary
)

SELECT
	salary_dollars,
	SUM(salary_dollars) OVER(
		-- including order by creates a cumulative/rolling sum starting at 
		-- first row in the window frame and ending at current row
		ORDER BY salary_dollars ASC
	)::FLOAT / (SELECT total FROM total_salary_spend)*100 AS cumulative_percent_total
FROM salary
ORDER BY salary_dollars ASC
```

#### Quartiles with lower and upper bound descriptors
- approach to provide additional context when generating quartiles

```
DROP TABLE IF EXISTS top_20_quarterbacks;
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

-- pretend list above is representative of all quarterbacks
WITH ntile_setup AS (
SELECT
	*,
	NTILE(4) OVER(ORDER BY total_td DESC) AS quartile
FROM top_20_quarterbacks
)

SELECT
	*,
	MIN(total_td) OVER(
		PARTITION BY quartile
	) AS quartile_lower_bound,
	MAX(total_td) OVER(
		PARTITION BY quartile
	) AS quartile_upper_bound
FROM ntile_setup
ORDER BY 
	total_td DESC, 
	id
```

#### Moving Average
- moving batting average across 3 seasons

```
DROP TABLE IF EXISTS temp_bonds;
CREATE TEMP TABLE temp_bonds (
  season int,
  batting_average numeric(4,3)
);

-- first 20 seasons stats for Barry Bonds
-- https://www.baseball-reference.com/players/b/bondsba01.shtml
INSERT INTO temp_bonds (season, batting_average)
VALUES
  (1986, 0.223),
  (1987, 0.261),
  (1988, 0.283),
  (1989, 0.248),
  (1990, 0.301),
  (1991, 0.292),
  (1992, 0.311),
  (1993, 0.336),
  (1994, 0.312),
  (1995, 0.294),
  (1996, 0.308),
  (1997, 0.291),
  (1998, 0.303),
  (1999, 0.262),
  (2000, 0.306),
  (2001, 0.328),
  (2002, 0.370),
  (2003, 0.341),
  (2004, 0.362),
  (2005, 0.286);

SELECT
	season,
	batting_average,
	-- logic to return NULL when fewer than 3 seasons
	CASE 
		WHEN season_order >= 3 
		THEN batting_avg_moving_average_3 
		ELSE NULL
	END AS batting_avg_moving_average_3_seasons 
FROM (
	SELECT
		*,
		ROW_NUMBER() OVER(
			ORDER BY season ASC
		) AS season_order,
		AVG(batting_average::FLOAT) OVER (
			ORDER BY season ASC
			ROWS BETWEEN 2 PRECEDING and CURRENT ROW
		) AS batting_avg_moving_average_3
	FROM temp_bonds
) AS sub_q 
```

#### New yearly revenue record
```
DROP TABLE IF EXISTS temp_revenue;
CREATE TEMP TABLE temp_revenue (
  year INTEGER,
  revenue NUMERIC(10, 2)
);

-- to get reproducible results
SELECT SETSEED(0.25);

INSERT INTO temp_revenue (year, revenue)
VALUES
  (1992, RANDOM() * 1000000.00),
  (1993, RANDOM() * 1200000.00),
  (1994, RANDOM() * 1500000.00),
  (1995, RANDOM() * 2000000.00),
  (1996, RANDOM() * 2500000.00),
  (1997, RANDOM() * 3000000.00),
  (1998, RANDOM() * 3500000.00),
  (1999, RANDOM() * 4000000.00),
  (2000, RANDOM() * 4500000.00),
  (2001, RANDOM() * 5000000.00),
  (2002, RANDOM() * 5500000.00),
  (2003, RANDOM() * 6000000.00),
  (2004, RANDOM() * 6500000.00),
  (2005, RANDOM() * 7000000.00),
  (2006, RANDOM() * 7500000.00),
  (2007, RANDOM() * 8000000.00),
  (2008, RANDOM() * 8500000.00),
  (2009, RANDOM() * 9000000.00),
  (2010, RANDOM() * 9500000.00),
  (2011, RANDOM() * 10000000.00),
  (2012, RANDOM() * 10500000.00),
  (2013, RANDOM() * 11000000.00),
  (2014, RANDOM() * 11500000.00),
  (2015, RANDOM() * 12000000.00),
  (2016, RANDOM() * 12500000.00),
  (2017, RANDOM() * 13000000.00),
  (2018, RANDOM() * 13500000.00),
  (2019, RANDOM() * 14000000.00),
  (2020, RANDOM() * 14500000.00),
  (2021, RANDOM() * 15000000.00);

WITH yearly_revenue AS (
SELECT
	year,
	revenue,
	MAX(revenue) OVER(
		ORDER BY year ASC
		ROWS BETWEEN UNBOUNDED PRECEDING AND 1 PRECEDING
	) AS previous_years_revenue_all_time_high
FROM temp_revenue
)

-- results expected to vary on each query run due to random function used above
SELECT
	*,
	CASE 
		WHEN revenue > previous_years_revenue_all_time_high
		THEN TRUE
		ELSE FALSE
	END AS new_all_time_revenue_high_flag
FROM yearly_revenue
```

#### Last 7 Day Total Sales
- note use of frame logic using RANGE BETWEEN which is conditional on date vs row position

```
DROP TABLE IF EXISTS temp_daily_sales;
CREATE TEMP TABLE temp_daily_sales AS

SELECT 
    DATE_TRUNC('day', sale_date) AS sales_day,
		ROUND((RANDOM()::NUMERIC * 1000),2) AS sale_amount
FROM GENERATE_SERIES(
        '2022-01-01'::timestamp, 
        '2022-04-04'::timestamp, 
        '1 day'::interval
    ) AS sale_date;

-- randomly drop rows to create intentional date gaps
DELETE FROM temp_daily_sales
WHERE RANDOM() < 0.3;

SELECT
	*,
	SUM(sale_amount) OVER (
      ORDER BY sales_day ASC 
      	-- handles use case when there are date gaps
		RANGE BETWEEN INTERVAL '6 days' PRECEDING AND CURRENT ROW
   ) AS last_7_days_sales	
FROM temp_daily_sales
```

#### Store Sales Performance
- ranking, ntiles, running total, etc

```
DROP TABLE IF EXISTS temp_store_sales_2022;
CREATE TEMP TABLE temp_store_sales_2022 (
  location_id INTEGER,
  sale_year INTEGER,
  total_sales NUMERIC(10,2)
);

SELECT SETSEED(0.5);

INSERT INTO temp_store_sales_2022 (location_id, sale_year, total_sales)
SELECT 
  location_id,
  2022::INT AS sales_year,
  ROUND(RANDOM()::NUMERIC * 100000, 2) AS total_sales
FROM 
  generate_series(1, 40) AS location_id;
  
SELECT 
	*,
	total_sales / SUM(total_sales) OVER () AS percent_total_sales,
	-- dense rank to allow for locations to have same rank if sales amount is equal
	DENSE_RANK() OVER(
		ORDER BY total_sales DESC
	) AS location_sales_rank,
	NTILE(10) OVER(
		ORDER BY total_sales DESC
	) AS location_sales_decile,
	SUM(total_sales) OVER(
		ORDER BY total_sales DESC
		ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
	) AS top_location_to_bottom_location_running_total,
	(
		SUM(total_sales) OVER (
			ORDER BY total_sales DESC
			ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
		) / 
		SUM(total_sales) OVER ()
	) * 100 AS cumulative_percent_total
FROM temp_store_sales_2022
ORDER BY total_sales DESC
```

#### YoY sales growth table
```
-- example output from a query to aggregate sales by year
WITH sales AS (
	SELECT 
		UNNEST(ARRAY[23456, 43567, 65812, 79234, 567394]) AS annual_sales,
		UNNEST(ARRAY[1999, 2000, 2001, 2002, 2003]) AS year
)

SELECT
	year,
	annual_sales,
	((annual_sales - LAG(annual_sales) OVER(ORDER BY year ASC))::FLOAT / annual_sales)*100 AS yoy_sales_growth
FROM sales
```

#### Sales by month and add YTD sales column
- note use of partition by year and frame boundary

```
WITH sales_data AS (
    SELECT
        '2022-10-01'::date AS sales_month,
        1000 AS sales
    UNION ALL
    SELECT
        '2022-11-01'::date,
        1200
    UNION ALL
    SELECT
        '2022-12-01'::date,
        1400
    UNION ALL
    SELECT
        '2023-01-01'::date,
        1100
)

SELECT
    sales_month,
    sales,
    SUM(sales) OVER (
        PARTITION BY EXTRACT(year FROM sales_month)
        ORDER BY sales_month
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS ytd_sales
FROM sales_data
ORDER BY sales_month;
```

#### Sales by month and add prior year sales column

```
WITH months AS (
SELECT
	generate_series(
		date_trunc('month', current_date - INTERVAL '2 years'),
		current_date,
		INTERVAL '1 month'
	) AS month_var
)

, example_sales_data AS (
SELECT
	month_var,
	ROUND((RANDOM() * 10000)::NUMERIC, 2) AS sales_amount
FROM months
)

SELECT
    *,
	-- NULLs expected for first year in example data
	-- use joins if gap in months expected
	LAG(sales_amount, 1) OVER (
        PARTITION BY EXTRACT(month FROM month_var)
        ORDER BY month_var
    ) AS prior_year_sales
FROM example_sales_data
WHERE month_var > first_month
ORDER BY month_var DESC;
```

#### Window function using GROUPS frame clause to create long vs wide output
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
- useful when dataset has long tails and there's a desire to reduce the influence of extreme outlier values on metrics
- example logic: if a value is below 10th percentile cap the value at the 10th percentile; if a value is above 90th percentile cap the value at the 90th percentile

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

# SQL Queries Best Practice Summary

1.  **Verify column value logic**

    -   Avoid assumptions on what column values represent (validate against ETL code, read documentation, consult peers, etc).

2.  **Sanity check query outputs**

    -   Avoid assuming data output is correct because a query ran successfully.

    -   Cross-check with manual calculations, other data sources, and know baseline/stable metrics.

3.  **Test logic on sample datasets**

    -   Validate logic on subset of data to catch issues early.

4.  **Handle NULL values strategically**

    -   Consider NULL value behavior in calculations and filters.

    -   Check for columns for NULL values: `COUNT(*) - COUNT(column_1) AS column_1_null_count`

5.  **Routinely monitor for unexpected values and dirty data**

    -   Monitor for data quality erosion (queries initially passing QA can later be affected by source data issues or bugs).
    -   Consider writing a set of QA tests for critical outputs.

6.  ***Avoid 'SELECT \*' outputs on large data***

    -   Specify columns to reduce output size.

    -   i.e. if downloading output to CSV, select specific columns of interest when possible vs `SELECT *` (especially important on wide and long tables when many columns will not be used for analysis).

7.  **Attempt to join on similar grain for large tables**

    -   i.e. 1 to 1 relationship joins between large tables tend to be more efficient vs 1 to many relationships.

    -   i.e. User_id 123 is one row in Table A and one row in Table B.

8.  **Include 'LIMIT X' when exploring data**

    -   Reduces load on database and allows for faster query execution.

    -   `SELECT order_id, amount FROM large_orders_table LIMIT 10`

9.  **Help your future self and colleagues with code comments/styling**

    -   Include inline code comments for complex logic.
    -   Follow an agreed upon style guide to assist with code readability speed.

10. **Include data filters early on**

    -   Reduce downstream processing load/run time by filtering to required data as early in query logic as possible.

\######################################################

Work in-prorgess

-   **Limit 'where' and 'join' logic on large data**

    -   Use efficient conditions and joins to minimize data processing.

    -   \^Describe this further; too vague.

-   Watch out for duplicate/dirty records in source data that can distort results

-   Look out for joins that cause unintentional fanout

-   Double check filter logic/join conditions

-   Timestamp time zones. Default tends to be UTC but different data sources and tables could have different time zones.

-   Alias sub query results by default to avoid errors.

-   SELECT 3 / 4 in PostgreSQL defaults to integer division which yields 0 instead of 0.75 as one might expect. SELECT ROUND(1.0 \* 3/4,2) returns 0.75.

-   Other best practices specific to data analysis SQL use cases

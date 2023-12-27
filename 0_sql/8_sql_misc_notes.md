# SQL Queries Best Practice Summary

1.  **Verify column value logic**

    -   Avoid assumptions on what column values represent (validate against ETL code or documentation).

2.  **Sanity check query outputs**

    -   Avoid assuming data output is correct because a query ran successfully.

    -   Test logic on small/fast datasets.

    -   Cross-check with manual calculations, other data sources, and know baseline/stable metrics.

3.  **Handle NULL values strategically**

    -   Consider NULLs in calculations and filters.

4.  **Monitor for unexpected values and dirty data**

    -   Monitor for data quality erosion; queries initially passing QA can later be affected by source data issues or bugs.

5.  ***Avoid 'select \*' outputs on large data***

    -   Specify columns to reduce output size.

6.  **Limit 'where' and 'join' logic on large data**

    -   Use efficient conditions and joins to minimize data processing.

7.  **Joins on similar grain for large data preferred**

    -   i.e. 1 to 1 relationship between tables. User_id 123 is one row in Table A and one row in Table B.

8.  **Include 'LIMIT X' when exploring data**

    -   Reduces load on database and allows for faster query execution.

Work in-prorgess

-   Watch out for duplicate/dirty records in source data that can distort results
-   Look out for joins that cause unintentional fanout
-   Double check filter logic/join conditions
-   Timestamp time zones. Default tends to be UTC but different data sources and tables could have different time zones.
-   Alias sub query results by default to avoid errors.
-   SELECT 3 / 4 in PostgreSQL defaults to integer division which yields 0 instead of 0.75 as one might expect. SELECT ROUND(1.0 \* 3/4,2) returns 0.75.
-   Include comments for complex logic
-   Follow style guide to assist with readability
-   Test on sample data first: (validate logic on subsets to identify issues early)

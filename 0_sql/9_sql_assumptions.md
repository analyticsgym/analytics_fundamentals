### SQL assumptions / misc notes

Before clicking run
- check table names are correct
- check metrics are correct
- commas after each select field

Verify the column values are what you'd expect. Can be risky assuming sales is daily sales simply because there is a daily timestamp and sales column.

Timestamp time zones. Default tends to be UTC but different data sources and tables could have different time zones.

-   avoid the temptation to just assume data is right just because a query ran
-   watch out for duplicate records in source data that can distort results
-   look out for joins that cause unintentional fanout
-   double check filter logic/join conditionss


Alias sub query results by default to avoid errors

PostgreSQL 
- defaults to floating point division in avg 
- Amazon Redshift defaults to integer division in avg

Reminder on CTR and rate problems to multiply by 100.0 

Queries to handle edge cases like division by zero and null values appropriately.

SELECT 3 / 4 in PostgreSQL defaults to integer division which yields 0 instead of 0.75 as one might expect.
SELECT ROUND(1.0 * 3/4,2) returns 0.75 

avoid the temptation to just assume data is right just because a query ran

watch out for duplicate records in source data that can distort results

look out for joins that cause unintentional fanout

double check filter logic/join conditionss
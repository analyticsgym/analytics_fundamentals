## TODOs
- Filtering in WHERE clause vs HAVING
- Add recs from ace the DS interview book page 145

## Query optimizations motivations
- queries that take a while to execute can limit analyst investigation areas and/or analysis depth
- overly complex queries on large datasets can bog down compute resources/become costly
- longer time windows between analyst question and insight discovery
- worth keeping some of these query optimization best practices top of mind
- test and compare alternative solutions to assess query speed/efficiency

## Best practice reminders

#### 1. Unions
- UNION vs UNION ALL: UNION tends to be more resource intensive on large datasets due to additional processing to remove duplicates
- if possible, use UNION ALL instead of UNION 
- single query joins or sub queries often preferred over UNION

#### 2. LEFT JOINs that return a large NULL set
- left joins on large datasets can be slow due to full scan required which returns NULLs for non-matches vs inner join grabs only matching records
- when the large NULL set isn't needed for the query requirements consider using inner join
- also applies to RIGHT JOINs (however, not used in the wild often)

#### 3. Pre-aggregate
- for large granular datasets it can be helpful to pre-aggregate at the individual table level before using them in joins (storing results as CTE or temp table)
- this can help improve performance by reducing the amount of data that needs to be processed during the join operation
- i.e. imagine 3 tables with user info/event data (demographics, community posts, video starts)
- pre-aggregation in this example could be aggregating community posts and video starts at the user level then joining to demographics at the same grain as user

#### 4. Subqueries 
- often best to avoid subqueries within joins that return large result sets
- subqueries within joins create an inefficient loop over the data where the subquery is executed repeatedly
- use CTEs or temp tables instead prior to join logic
- also best to avoid window functions in sub queries (use intermediate CTE or temp table to store results)

#### 5. Filtering location
- when possible, apply filtering conditions as early in the query structure as possible to minimize amount of data to process
- for example, when feasible, including filtering conditions as part of inner join logic vs where clause

#### 6. Window functions
- filter data only to required rows to avoid unnecessary compute
- grouping data into smaller partitions might help query efficiency/speed
- nuances of the data can impact window function speed (i.e. number of ntiles, large offsets for lead/lag, large window frames, many rows with duplicate values, etc)
- general themes on window function speed: tend to be fastest (ROW_NUMBER, RANK, DENSE_RANK), moderate speed (LAG, LEAD, FIRST_VALUE, LAST_VALUE), can be slower (NTILE, SUM, AVG, MIN, MAX, COUNT) 

#### 7. Downloading data from query UI 
- for wide datasets, selecting specific columns to output to file vs select * can be faster and more readable for future code users
- also throws an error if the table columns ever change after specification

#### 8. Exploration phase
- include LIMITs during initial exploration of tables (e.g. prevents hanging queries)
- create a small subset of data to test queries and logic on
- for prototyping of techniques or testing functions consider generating a fake MVP dataset (e.g. create a TEMP TABLE and insert values, UNION ALL values together, etc)

#### 9. Others???
- ...

## Not tested in the wild

#### 1. Indexing
- is a way to improve query performance by creating organizational data structure to more quickly locate data (i.e. think a file cabinet of folders with tab labels)
- haven't had experience implementing this optimization approach
- several types of indexing exist in PostgresSQL (i.e. B-tree, GiST, GIN indexs)
- data engineers might use this lever behind the scenes when building prod tables

#### 2. Query planner
- useful when there is a need to optimize query performance
- can be used to compare performance between queries
- EXPLAIN command used to generate query plan for performance comparison and query cost/resource estimation
- haven't used this technique on actual work projects

## Misc nots

#### Statistical sorting functions on large datasets (e.g. median, percentile)
- these functions can be slow on large datasets due to sorting and scanning large amounts of data
- skewed distributions can also take longer to calculate
- ideas to speed up query: data sampling, partitioning data, creating indexes on the calculation column 


JOIN best practices

<<< REVISE >>>

inner joins tend to be more resoure efficient/faster than outer joins (left, right, outer)

if an inner join works for the use case then default to using inner joins

<aggregation before joins to create a 1 to 1 join>

<joining at hgih

avoid subquiries in join conditions

use ON clause for join conditions vs where clause

limit the number of joins to only the required tables

# Starrocks

An Open-Source, High-Performance Analytical Database
* StarRocks has 2 main components - frontends (FEs) and backends (BEs).
* FEs and BEs can be horizontally scaled without service downtime.
* It is a column-oriented database system.
* FEs are responsible for metadata management, client connection management, query planning, and query scheduling. Parses query into a logical plan.
* BEs are responsible for data storage and SQL execution. Logical plan to physical execution plan.
* It is compatible with MySQL protocols and supports standard SQL. Users can easily connect to StarRocks from MySQL clients.
* Materialized views supported - it will automatically check if creating a materialized view would make the query run faster. If so, it will create one for us in the background without needing to do anything. We can also create or delete materialized views as needed based on our business needs.
* It has mechanism for metadata and service data replication, which increases data reliability and prevents single points of failure (SPOFs).
* For more details, https://docs.starrocks.io/en-us/3.0/introduction/Architecture 
* https://docs.starrocks.io/en-us/latest/quick_start/Deploy 
* Base hardware - machine with 4 CPU cores and 16 GB of RAM. The CPU MUST support AVX2 instruction sets. The M1 CPU does not support AVX (Advanced Vector Extensions) or AVX2 instructions, which are commonly used in high-performance computing applications such as video and audio editing, 3D rendering, and scientific simulations.
* Base Software - machine MUST be running on OS with Linux kernel 3.10 or later
* https://docs.starrocks.io/en-us/latest/table_design/StarRocks_table_design

## Tables
* Like in other relational databases, tables in StarRocks consist of rows and columns. Each row holds a record of user data, and data in each column has the same type. All rows in a table have the same number of columns. 
* You can dynamically add columns to or delete columns from a table. 
* The columns of a table can be categorized as dimension columns and metric columns. 
* Dimension columns are also called key columns.
* Values in dimension columns are used to group and sort data. 
* Metric columns are also called value columns. 
* Values in metric columns can be accumulated by using functions such as sum, count, min, max, hll_union_agg, and bitmap_union.

## DML and DDL
```sql
select count(*) from  dbname.employee;
show tables;
SHOW FULL COLUMNS FROM employee;
show create table employee;
SHOW TABLE STATUS like 'employee'; # returns the row count,  avg column size etc.

```
* DDL - [CREATE TABLE @ CREATE TABLE ](https://docs.starrocks.io/en-us/2.3/sql-reference/sql-statements/data-definition/CREATE%20TABLE)
* DML - [ SELECT @ SELECT ](https://docs.starrocks.io/en-us/2.3/sql-reference/sql-statements/data-manipulation/SELECT)
* [Data type overview @ data-type-list ](https://docs.starrocks.io/en-us/latest/sql-reference/sql-statements/data-types/data-type-list)
* [Create a table @ Create_table ](https://docs.starrocks.io/en-us/3.0/quick_start/Create_table)
* [Bucketing Rules](https://docs.starrocks.io/en-us/3.0/table_design/Data_distribution#design-partitioning-and-bucketing-rules)
  * Partitioning column. In StarRocks, only the column of the DATE, DATETIME or INT type can be used as the partitioning column, ones with low cardinality and used as a filter in queries. Low-Cardinality: Refers to values that are common within the data set and have very few possible values. Examples include status, gender, and booleans. 
  * hash/bucketing column. Data in partitions can be subdivided into tablets based on the hash values of the bucketing columns and the number of buckets. Bucketing columns are high cardinality column such as ID  and used as a filter in queries, The data types of bucketing columns must be INTEGER, DECIMAL, DATE/DATETIME, or CHAR/VARCHAR/STRING.
      * Bucketing columns cannot be modified after they are specified.
      * Mulitple columns can be used for bucketing

## Query Optimization
*[ Gather statistics for CBO @ Cost_based_optimizer](https://docs.starrocks.io/en-us/latest/using_starrocks/Cost_based_optimizer) . CBO is enabled by default  from StarRocks 1.19 onwards. Not sure if the rules bases optimization is used any more. I assume we use the Auto mode and do not force statistics collection.
* Materialized views
    * [Synchronous materialized view @ Materialized_view-single_table ](https://docs.starrocks.io/en-us/latest/using_starrocks/Materialized_view-single_table)
    * [Asynchronous materialized views @ Materialized_view ](https://docs.starrocks.io/en-us/latest/using_starrocks/Materialized_view)
  


## Reference:
- https://www.starrocks.io/ 
- https://medium.com/@rajnishkumargarg/starrocks-introduction-f9026fadd68b

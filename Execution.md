Execution
===============

### Spark Execution:
- Write DataFrame/Dataset/SQL Code
- If the code is valid, Spark converts it to a 'Logical Plan' (set of abstract transformations that do not refer to executors or drivers.)
- Spark transforms this 'Logical Plan' into a 'Physical Plan'
- Spark then executes this 'Physical Plan' on the cluster

#### Logical Planning:
- User code into an unresolved logical plan (code may be valid, the tables or columns that it refers to may or may not exist)
- Analyzer needs catalog (a repository of all table and DataFrame information) in order to resolve columns and tables of an unresolved logical plan.
- The resolved logical plan goes through optimizer and it creates an optimized logical plan.
#### Physical Planning:
- specifies how the logical plan will execute on the cluster by generating different physical execution strategies and comparing them through a cost model. It compiles DataFrames, ... into RDD transformations.

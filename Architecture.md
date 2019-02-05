Architecture
=============
master/worker architecture

### Basic architecture
* **Cluster** - a group of machines. One spark cluster can run several Spark applications.
* **Spark application** - consists of driver process and executors processes.
* **Driver process** - runs on Driver node and maintains information about Spark app, responses user's program, analyzing, distributing and scheduling work for executors. It is an essential part of Spark (the heart of a spark app).
* **Executor** - is responsible for executing code and reporting the state back to the driver.
* **Cluster Manager** - controls physical machines and allocate resources to Spark application. Spark supports three cluster managers: Standalone Cluster Manager, [Apache Mesos](https://mesos.apache.org/), and [Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html). Thanks to that, there can be multiple Spark applications running at same time. If you use the Standalone Cluster Manager then installing Spark on each node of the cluster is required.
* Spark can also run in **local mode**. Processes run on local machine instead of a cluster -> it is the easiest way to run Spark.
* **SparkSession** - It is the entrance point to execute code in Spark.
* **SparkContext** is the entry point for low-level API functionality. You access it through the SparkSession.
* **DataFrame** - It is a table of data with rows and columns that are defined by a schema. DataFrame has zero or more partitions (data can be on more computers). When the DataFrame is created the transformation is performed.
* **Partition** - It is a collection of rows that sit on one physical machine in the cluster. Spark will operate on each partition in parallel unless an operation calls for a shuffle, where multiple partitions need to share data.
  * The partitioning scheme defines how that is broken up, this can be set to be based on values in a certain column or non-deterministically.


### Partitioning
- goal ->  evenly spread our keys
- With good partitioning, you can avoid lots of data transfering
- Number of partition is configurable. By default, it equals a number of cores on all executors nodes.
- Two types of partitioning:
  - **hash part.** - `p = k.hashCode() % numPartitions`
  - **range part.** - ordering for keys, sorted ranges keys
- Two ways to create RDD with specific partitioning:
  - Mathod `parititonBy` on RDD with specific partitioner (`RangePartitioner`)
  - Using transformation that returns RDD with specific transformation


### Spark Execution
- Write DataFrame/Dataset/SQL Code
- If the code is valid, Spark converts it to a 'Logical Plan' (set of abstract transformations that do not refer to executors or drivers.)
- Spark transforms this 'Logical Plan' into a 'Physical Plan'
- Spark then executes this 'Physical Plan' on the cluster

#### Resource Allocation
- static allocation - the finite maximum resource is hardly set
- dynamic allocation - executors are added and removed from a Spark app as needed

#### Logical Planning
- User code into an unresolved logical plan (code may be valid, the tables or columns that it refers to may or may not exist)
- Analyzer needs catalog (a repository of all table and DataFrame information) in order to resolve columns and tables of an unresolved logical plan.
- The resolved logical plan goes through optimizer and it creates an optimized logical plan.

#### Physical Planning
- specifies how the logical plan will execute on the cluster by generating different physical execution strategies and comparing them through a cost model. It compiles DataFrames, ... into RDD transformations.

#### Run the start
- Spark application begins when a `SparkContext` is started. Then the driver and executors (on its own JVM) are started on the workers node. One node can have more executors.

#### Scheduling
- Spark schedules jobs on a fifo basis, by default.
- With each action the Scheduler builds execution graph and launch the spark job.
- **Spark Job**
  - Each spark job corresponds to one action
  - The highest element of execution hierarchy
- **Stages**
  - Set of computations (tasks) that can be computed on one executor without communication with others nodes.
  - Wide transformations define breakdowns job into stages.
  - New stage begins whenever network communication is necessary between nodes
  - Transformations with narrow dependency can be grouped into one stage
- **Tasks**
  - (combine narrow transformation)
  - Task represent parallel computing and is performer on executor.
  - Task can not be executed on more than one executor.
  - Number of task per stage corresponds to the number of partition in output RDD of the stage
- **DAG**
  - builds a graph of stages for each job
  
### Optimizer
  - Catalyst is the Spark SQL query optimizer.
  - It takes query plan (logical plan) and transform it into an execution plan that Spark can run.
  - Spark is able to apply optimalization on the logical plan and choose between multiple physical plan using a cost-based model.

Architecture
=============

### Basic architecture
* **Cluster** - a group of machines
* **Spark application** - consists of driver process and executors processes.
* **Driver process** - runs on Driver node and maintains information about Spark app, responses user's program, analyzing, distributing and scheduling work for executors. It is an essential part of Spark (the heart of a spark app).
* **Executor** - is responsible for executing code and reporting the state back to the driver.
* **Cluster Manager** - controls physical machines and allocate resources to Spark application. Spark supports three cluster managers: Standalone Cluster Manager, [Apache Mesos](), and [Hadoop YARN](). Thanks to that, there can be multiple Spark applications running at same time. If you use the Standalone Cluster Manager then installing Spark on each node of the cluster is required.
* Spark can also run in **local mode**. Processes run on local machine instead of a cluster -> it is the easiest way to run Spark.
* **SparkSession** - It is the entrance point to execute code in Spark.
* **DataFrame** - It is a table of data with rows and columns that are defined by a schema. DataFrame has zero or more partitions (data can be on more computers). When the DataFrame is created the transformation is performed.
* **Partition** - It is a collection of rows that sit on one physical machine in the cluster. Spark will operate on each partition in parallel unless an operation calls for a shuffle, where multiple partitions need to share data.
  * The partitioning scheme defines how that is broken up, this can be set to be based on values in a certain column or non-deterministically.

Introduction
=============

Spark is a distributed programming model where the user specifies [transformations](), which build up a directed-acyclic-graph of instructions, and actions, which begin the process of executing that graph of instructions, as a single job, by breaking it down into stages and tasks to execute across the cluster. (note: quite complicated right :D )

It is more than computing model but an ecosystem of a variety of packages, libraries, and systems built on top of the Core of Spark.

### Basics
* The first stable version of [Apache Spark](https://spark.apache.org/) was released in May 2014
* Spark is primarily written in Scala and can be used also with Java, Python, R (and SQL).
* Spark is a framework for managing and coordinating the resources of a cluster for the goal to process a huge amount of data.
* Spark has two API - The [Unstructured]() and [Structured]()
* Spark has several core abstractions: [Datasets](), [DataFrames](), [SQL Tables](), and [Resilient Distributed Datasets]() (RDDs). These abstractions all represent distributed collections of data however they have different interfaces for working with that data.
  * The Unstructured API is lower level API, includes RDD, [Accumulators]() and [Broadcast variables](). It is mainly for manipulation of raw Java objects.
  * The Structured API includes DataFrames, Datasets, Spark SQL. Is the interface that most users should use. It does some optimization work with structured data. It organizes data into the table.
* Spark organizes computation into two categories - [transformations]() and [actions]().
* Core Spark data structures are immutable.
* Lazy evaluation - Spark waits until the very last moment to execute your transformations. It allows combine operation that doesn't require communications with driver.
* Fault-tolerance - Spark will not fail, lose data, or return inaccurate results in the event of a host machine or network failure.
* Memory management - There are three options for memory management: in-memory deserialized, in-memory serialized, and on the disk.
* Spark console - `./bin/spark-shell`

### Spark components
Outside of Spark Core, there are other components: SparkSQL, MLlib, Spark Streaming, GraphX.

### Spark UI
* You can monitor the progress of their job through the Spark UI that is available at http://localhost:4040 if you run in local mode or at the driver node.

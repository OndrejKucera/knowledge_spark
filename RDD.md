RDD
======================

* It represents a large dataset.
* It is immutable distributed data collection (statically typed) of objects which are stored in the executors (slave nodes). Object that comprise RDD are called partitions
* All RDDs are evaluated lazily
* It has a number of predefined transformations, (`map`, `join`, `reduce`, ...) that are similar as in Scala, to manipulate the distributed datasets.
* Spark can keep loaded RDD in the memory on the executor nodes through the life of Spark application (faster access in repeat computation)
* There are three ways how is possible to create RDD
  * by transforming an existing RDD
  * from SparkContext by `makeRDD` or `parallelize` methods. Or reading from a stable storage
  * converting DataFrame or Dataset (created from SparkSession) 
* Some operations have a different signature in Java than in Scala
* The `toDebugString` for find out the type of RDD

### Functions on RDDs
Two types, the transformation and action

#### Action
- It is an operation that returns something other than RDD, including side effects
- The computation doesn't start until action is called. Each Spark application has to contain the action. Since action brings either information back to the driver or writes data to stable storage
- It triggers (forced evaluation) the scheduler, which builds a direct acyclic graph (DAG)
- Some of the action doesn't scale well and can cause memory errors in the driver. It is better to use action like `take`, `count`, `reduce` which brings back a fixed amount of data rather than `collect` and `sample`
- The `foreach` can be used to force evaluation of RDD but is also used to write unsupported format like web endpoints.
- There are three types of actions:
 - for viewing data in the console
 - to collect data to native objects
 - for writing data to output source

#### Transformation
- It always returns another RDD
- Two type of transformation: with narrow dependencies and with wide dependencies
  - **narrow** - It can depend on the parent (as map), or a unique subset of the parent partitions that is known at design time (coalesce) -> no need any information from other partitions 
  - **wide** - It includes `sort`,`reduceByKey`, `join`, and anything that calls the repartitions function. -> it will cause an expensive shuffle. If Spark already knows that data are partitioned in a certain way, operations with wide dep. do not cause shuffle.
- Not all transformations are 100% lazy evaluated, i.e. `sortByKey` needs to evaluate RDD

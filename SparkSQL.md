Spark SQL - Structured API
====================

Structured API refer to operations on `DataFrames`, `Datasets`, and in SQL-like. Most operations apply both batch and stream computation.

Two distinct in the Structured APIs: **DataFrames and Datasets** are distributed tables with rows and columns. Columns have type information.

**The plan** specifies how to perform a series of transformations over DataFrame to generate the correct output. When we perform an action on a DataFrame we instruct Spark to perform the actual transformations that represent that DataFrame.

### Schema:
* One core concept that differentiates the Structured APIs from the lower level APIs.
* A schema defines the column names and types of a DataFrame.
* It can be defined by the data source or user can define it.
* A schema is a _StructType_ made up of a number of fields, _StructFields_, that have a name, type, and a boolean flag.

### Overview of Structured Spark Types:
- **DataFrames** 
  * Untyped API, goes across languages. The 'untyped API' does have types but it only operates on Spark types at runtime.
- **Datasets**
  * typed API, only available to JVM based languages (Scala and Java). The “typed API” allows you to define your own types to represent each record in your dataset with “case classes or Java Beans” and types are checked at compile time.
- **Columns**
  * They can represent a simple type like an integer or string, complex types like an array or map, or a null value.
  * We can select, manipulate, and remove columns from DataFrames.
  * We can construct and or refer to columns with the functions _col_ or _column_ in package _import org.apache.spark.sql.functions._
  * To refer to a specific DataFrame’s column, we can use the _col_ method on the specific _DataFrame_. (i.e. df.col("count"))
  * Columns are not resolved until we compare the column names with those we are maintaining in the catalog.
  * When you want to get DataFrame’s columns then use _columns_ method, df.columns()
- **Expressions**
  * Expressions are columns. _expr("someCol")_ is is equivalent to _col("someCol")_
  * Columns provide a subset of expressions functionality. 
  * It is set of transformations on records in DataFrames.
- **Rows** 
  * Row objects are the most general way of getting data into, and out of, Spark and are available in all languages. Each record in a DataFrame must be of Row type.
  * Row has no schema
  * Row objects are the objects that column expressions operate on to produce some usable value.
  * We can get Row by call method first, _df.first()_
- **Spark Value Types**
  * Basic types are mapped into the spark types.
- **Encoders**
  * Using Spark from Scala and Java allows you to define your own JVM types to use in place of Rows that consist of the above data types.

====
* There is no performance difference between writing SQL queries or writing DataFrame code.
Note: Method Dataset.explain() - Prints the physical plan to the console for debugging purposes.

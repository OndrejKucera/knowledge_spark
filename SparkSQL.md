Spark SQL
====================

Spark SQL is module for **structured data processing**. It contents the `DataFrames`, `Datasets`, and in SQL-like operations. Most operations apply both batch and stream computation.

DataFrames and Datasets represent Distributed collections (with rows and columns -> type information), with additional schema information not found in RDD.

Three main goals:
  - support relation processing
  - high performance -> Backend components: Catalyst (optimizer), Tungsten (off-heap serializer)
  - support data sources -> semi-structural data and external databases
  
Note: There is no performance difference between writing SQL queries or writing DataFrame code. `Method Dataset.explain()` prints the physical plan to the console for debugging purposes.

### Schema:
* One core concept that differentiates the Structured APIs from the lower level APIs.
* A schema defines the column names and types of a DataFrame.
* It can be defined by the data source or user can define it.
* A schema is a `StructType` made up of a number of fields, `StructFields` that have a name, type, and a boolean flag.
* With Schema, it is possible to provide more efficient storage layer (Tungsten) and also more optimalization (Catalyst)

### Structured Spark Types:
- **DataFrames** 
  * Untyped API, goes across languages. The 'untyped API' does have types but it only operates on Spark types at runtime.
- **Datasets**
  * Typed API, only available to JVM based languages (Scala and Java). The “typed API” allows you to define your own types to represent each record in your dataset with “case classes or Java Beans” and types are checked at compile time.
- **Columns**
  * They can represent a simple type like an integer or string, complex types like an array or map, or a null value.
  * We can select, manipulate, and remove columns from DataFrames.
  * We can construct and or refer to columns with the functions `col` or `column` in package `import org.apache.spark.sql.functions._`
  * To refer to a specific DataFrame’s column, we can use the `col` method on the specific `DataFrame`. (i.e. df.col("count"))
  * Columns are not resolved until we compare the column names with those we are maintaining in the catalog.
  * When you want to get DataFrame’s columns then use method `df.columns()`
- **Expressions**
  * Expressions are columns. `expr("someCol")` is is equivalent to `col("someCol")`
  * Columns provide a subset of expressions functionality. 
  * It is set of transformations on records in DataFrames.
- **Rows** 
  * Row objects are the most general way of getting data into, and out of, Spark and are available in all languages. Each record in a DataFrame must be of Row type.
  * Row has no schema!
  * Row objects are the objects that column expressions operate on to produce some usable value.
  * We can get Row by call method first `df.first()`
- **Spark Value Types**
  * Basic Scala types are mapped into the Spark SQL types. i.e. `Int` -> `IntegerType`, `Array[T]` -> `ArrayType(elementType, containsNull)`, `case class` -> `StructType(List(StructFileds))`
- **Encoders**
  * Using Spark from Scala and Java allows you to define your own JVM types to use in place of Rows that consist of the above data types.

### DataFrames
- DataFrame is Untyped (goes across languages). Scala compiler doesn't (and can't) check the types in the schema in DataFrames -> Limitation
- DataFrames are Dataset of a special Row object, which is not parametrized by a type `type DataFrame = Dataset[Row]`
- The 'untyped API' does have types. It only operates on Spark types at runtime.
- DataFrame is SQL's core abstraction -> RDD with Schema which is required -> conceptional as table in SQL
- How to create DataFrame:
  - from existing RDD `rdd.toDF("id", "name")`, `peopleRDD.toDF` or `spark.createDataFrame(rowRDD, schema)`
  - reading data source from file (json, ...) `spark.read.json("path")`
- If you want to use sql query you have to register the table `spark.sql("... some query ...")`
- **Operations**
  - Transformation returns DataFrame and are lazy
  - `select`, `agg`, `where`, `limit`, `filter`, `orderBy`, `groupBy`, `join`, `union`
  - `printSchema` -> print schema of DataFrame
  - i.e. `df.filter($"age" > 18)`
  - **Grouping & Aggregating**
    - groupBy returns `RelationalGroupedDataset`. It has several aggrgation functions `sum`, `max`, `min`, `count` ...
  - **Dropping/Repleacing columns** 
    - `drop` - drop rows that contains null or Nan in any column or `drop(Array("id", "age"))` in specific columns
    - `fill(0)` - replace all accurrences of null or Nan with specific value
  - **Joins**
    - `df1.join(df2, $"df1.id" === $"df2.id")`
    - There many types of join -> inner, outer, left_outer, right_outer, leftsemi -> `df1.join(df2, $"df1.id" === $"df2.id", "right_outer")`
- **Actions**
  - `collect`, `count`, `show` (pretty prints of DataFrame), `first`, `head`, `take`

### Dataset
  - Typed distributed collection of data
  - It requires structured/semi-structed data
  - unifies DataFrame and RDD API -> type information + optimalization
  - It is possible to use relation DataFrame operations on the `Dataset` and also higher-order functions like map, flatMap
  - creatign Dataset:
    - call `toDS` on Dataframe, RDD, or Scala collections (implicits)
  - It has `TypedColumn` -> `$"price".as[Double]`
  - It has two types of transformation: 
    - typed
      - `map`, `flatMap`, `filter`, `distinct`, `coalesce`, `repartition`
      - `groupByKey` returns `KeyValueGroupDataset[K,T]` aggregation operation `agg` can be called which return Dataset. Or `reduceGroups`, `mapGroups`, `flatMapGroups`
    - untyped
  - There are also transformation for working with multiple datasets - `intersect`, `union`, `substract`
  - Actions: same as in RDDs and DataFrames

### Deferences between APIs
 - **Dataset**
   - semi-structed/structed data
   - typesafety
   - functional API
   - good performace (but not the best -> you can miss some optimalization i.e. when you use functional style)
 - **DataFrame**
   - semi-structed/structed data
   - the best possible performace 
 - **RDD**
   - you have unstructured data
   - you want fine-tuned and manage low-level data
   - you have complex data that can not be serialized with Encoder
   
### User-Defined Functions (UDF or UDAF)
  - way to extend DataFrame and SQL with custom code while you keep the Catalyst optimizer
  - It is usefull for Dataset because otherwise you would need to transform data to RDD which is expensive.
  - **UDF** - Writing UDF for Spark SQL is simple. It is like regular function which is registred by using `sqlContect.udf().register("strlen", (s: String) => s.length())`
  - **UDAF** - Aggregate functions are bit more complicated. The `UserDefinedAggregateFunction` has to be extended. UDAF can be more performant compare to `mapGroups` on Dataset.

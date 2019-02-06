Join
=============
Join is one of most expensive operation

- The join may require large network transfers (shuffles) or even the result dataset could be beyond storing capabilities.    

#### RDD type join
- both keys have to be located in same partition

- best case for standard join is when both RDDs have same set of distinct keys.
- with duplicate keys, the data get bigger and perforamnce can get slower
  - It is better to perform `distinct` or `combineByKey` for reducing key space
  - `cogroup` to handle duplicate keys
- if a key is not present in one of the set them the item will be lost.
  - It is safer to use outer join and then filter data after join
- if one of the set has small subset of keys it could be better to `reduce` or `filter` before the join and avoid huge shuffle

- execution plan -> default shuffle during the join uses shuffle hash join -> but shuffle can be avoided
  - if borh RDD have same partitioner
  - one of the dataset is small enough fit into memory -> broadcast hash join

#### Spark SQL join

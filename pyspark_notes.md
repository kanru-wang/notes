### pyspark.sql.functions

- `.col()` Return a Column based on the given column name
- `.trim()` Trim the spaces from both ends for the specified string column
- `.concat_ws()` Concatenate multiple input string columns together into a single string column, using the given separator
- `.lit()` Create a Column of literal value
- `.udf(lambda x: ...)` Create a user defined function. Every time want "lambda" should use this.
- `.when().otherwise().alias("col_name")`
  - `df.select(when(df["col_a"] == 2, 3).otherwise(4).alias(“col_b”))`
  - `df.withColumn("col_a", (when(df["col_a"] == 2, 3).otherwise(4))`
- `f.date_format(f.col("date_col"), "yyyyMMdd")`
- `df.select(arrays_zip("col_1", "col_2").alias("zipped"))`
- `explode` Similar to `pandas.DataFrame.explode` 

### pyspark.sql.DataFrame

- `.dropDuplicates()`
- `.withColumn()` Add a column
- `.select()` Select certain columns and return a new DataFrame
- `.withColumnRenamed()`
- `.drop()`
- `.filter()` Filters rows. `where()` is an alias for `filter()`.
- `.createOrReplaceTempView()` The lifetime of this temp table is tied to the SparkSession
- `.union()` Concat two dataframes up and down. Resolves columns by position (not name)
- `.replace()` Replace all occurances of a certain value in the df to another value
- `.repartition()` If argument is an int, will specify the number of partitions; if is a col name, will be used as the first partitioning column
  - `df.repartition(1).write.partitionBy("optional_partitionBy").mode("overwrite").parquet(some_path)`
  - `repartition(1)` and `coalesce(1)` can be used to write out a DataFrame to a single file. Avoid writing out a DataFrame to a single file because it is slow and has a size limit. Only write out a DataFrame to a single file when it is tiny.
- `.printSchema()`

### pyspark.sql.types

    from pyspark.sql.types import StructType, StructField, StringType, ArrayType
    data = [("Alice", ["Java", "Scala"]), ("Bob", ["Python", "Scala"])]
    schema = StructType([
        StructField("name", StringType()),
        StructField("languagesSkills", ArrayType(StringType())),
    ])
    df = spark.createDataFrame(data=data, schema=schema)

### Others

- `pyspark.sql.Column.isin()` If each value in a column is in a list of values
- `pyspark.sql.Window`
  - `.withColumn("total_aaa", f.sum("aaa").over(Window.partitionBy("some_id")))`
  - `.withColumn("rank", rank().over(Window.partitionBy("some_id")))`
- `map(lambda x: )`
- `reduceByKey` to count occurrences
- `sortByKey`
- `keyBy` and `join`
- `broadcast` a dictionary and then lookup the wanted value by the dictionary key, will result in the same effect of joining
- `accumulator` to calculate the global average
- `pyspark.sql.GroupedData.pivot` Similar to `pandas.DataFrame.pivot`
  
<br>
<br>

### Tips

- Repartition
  - More repartitioning is not necessarily better, because repartitioning involves shuffling data across nodes, which is an overhead cost. Also, there is an overhead cost to manage partitions.
  - A rule of thumb is to have 2-3 partitions for each CPU core in a cluster. Optimal partition size is often between 128MB and 256MB.
  - Where and how often to use repartition
    - At the beginning, to distribute the data evenly across a cluster.
    - Before `groupBy`, `join`, or aggregations, that will cause a lot of shuffling or result in skewed data.
    - After significantly reducing the data volume through filtering or aggregation, repartition the data can reduce the number of partitions and avoid overhead from too many small partitions.
- Persist / Cache
  - If repartition and then perform multiple actions on the DataFrame, it might be beneficial to persist (cache) the DataFrame. Because actions cause the entire transformation pipeline to be executed, and without caching, repartitioning would be done repeatedly.
    ```
        df = df.repartition(10)
        df.cache()
        df.count()
        df.show()
    ```
  - `.unpersist()` when it's no longer needed to free up resources.
- Use cases for `coalesce`
  - `coalesce` is used to decrease the number of partitions in a DataFrame.
  - It avoids data shuffle, making it more efficient than `repartition` for reducing partitions.
  - It's particularly useful after operations that filter the data, leading to underutilized (sparsely populated) partitions.
  - Use it to reduce the number of partitions without a full shuffle of the data across the cluster.
- Joins
  - Use broadcast joins for small datasets.
    - "broadcast" or send a copy of the smaller DataFrame or table to all nodes in the cluster, so it's available in the memory of each node. This way, when performing the join, Spark doesn't need to shuffle the larger DataFrame across the network, significantly reducing the amount of data transfer and the overall time it takes to complete the join operation.
    - Spark can automatically decide to use a broadcast join if the `spark.sql.autoBroadcastJoinThreshold` configuration parameter is set and the smaller DataFrame is below this size threshold.
    - For lookups against a large dataset in a UDF or a map operation, use broadcast variables to distribute the dataset to all nodes efficiently.
  - When joining datasets in PySpark, ensure they are partitioned on the join key.
- Costly operations
  - `map`, `filter`, and `union` can often be performed without shuffling data across partitions. Should structure transformations to maximize the use of these operations.
  - `reduceByKey`, `groupBy`, `join` would cause shuffling.

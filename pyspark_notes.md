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
  

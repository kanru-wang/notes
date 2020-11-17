### pyspark.sql.functions

- .col() Return a Column based on the given column name
- .trim() Trim the spaces from both ends for the specified string column
- .concat_ws() Concatenate multiple input string columns together into a single string column, using the given separator
- .lit() Create a Column of literal value
- .udf(lambda x: ...) Create a user defined function. Every time want "lambda" should use this.
- .when().otherwise()

### pyspark.sql.DataFrame

- .dropDuplicates()
- .withColumn() Add a column
- .select() Select certain columns and return a new DataFrame
- .withColumnRenamed()
- .drop()
- .filter() Filters rows. where() is an alias for filter().
- .createOrReplaceTempView() The lifetime of this temp table is tied to the SparkSession
- .union() Concat two dataframes up and down. Resolves columns by position (not name)
- .replace() Replace all occurances of a certain value in the df to another value

### Others

- pyspark.sql.Column.isin() If each value in a column is in a list of values
- `map(lambda x: )`
- `reduceByKey` to count occurrences
- `sortByKey`
- `keyBy` and `join`
- `broadcast` a dictionary and then lookup the wanted value by the dictionary key, will result in the same effect of joining
- `accumulator` to calculate the global average
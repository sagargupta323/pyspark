PySpark Cheat Sheet
===================
This cheat sheet will help you learn PySpark and write PySpark apps faster. Everything in here is fully functional PySpark code you can run or adapt to your programs.

These snippets are licensed under the CC0 1.0 Universal License. That means you can freely copy and adapt these code snippets and you don't need to give attribution or include any notices.

These snippets use DataFrames loaded from various data sources:
- "Auto MPG Data Set" available from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/auto+mpg).
- customer_spend.csv, a generated time series dataset.
- date_examples.csv, a generated dataset with various date and time formats.

These snippets were tested against the Spark 3.2.1 API. This page was last updated 2022-03-13 08:55:24.

Make note of these helpful links:
- [PySpark DataFrame Operations](http://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.html#dataframe-apis)
- [Built-in Spark SQL Functions](https://spark.apache.org/docs/latest/api/sql/index.html)
- [MLlib Main Guide](http://spark.apache.org/docs/latest/ml-guide.html)
- [Structured Streaming Guide](https://spark.apache.org/docs/latest/api/python/reference/pyspark.ss.html)
- [PySpark SQL Functions Source](https://spark.apache.org/docs/latest/api/python/_modules/pyspark/sql/functions.html)

Try in a Notebook
-----------------
See the [Notebook How-To](notebook.md) for instructions on running in a Jupyter notebook.

Generate the Cheatsheet
-----------------------
You can generate the cheatsheet by running `cheatsheet.py` in your PySpark environment as follows:
- Install dependencies: `pip3 install -r requirements.txt`
- Generate README.md: `python3 cheatsheet.py`
- Generate cheatsheet.ipynb: `python3 cheatsheet.py --notebook`


Table of contents
=================

<!--ts-->
   * [Accessing Data Sources](#accessing-data-sources)
      * [Load a DataFrame from CSV](#load-a-dataframe-from-csv)
      * [Load a DataFrame from a Tab Separated Value (TSV) file](#load-a-dataframe-from-a-tab-separated-value-tsv-file)
      * [Save a DataFrame in CSV format](#save-a-dataframe-in-csv-format)
      * [Load a DataFrame from Parquet](#load-a-dataframe-from-parquet)
      * [Save a DataFrame in Parquet format](#save-a-dataframe-in-parquet-format)
      * [Load a DataFrame from JSON Lines (jsonl) Formatted Data](#load-a-dataframe-from-json-lines-jsonl-formatted-data)
      * [Save a DataFrame into a Hive catalog table](#save-a-dataframe-into-a-hive-catalog-table)
      * [Load a Hive catalog table into a DataFrame](#load-a-hive-catalog-table-into-a-dataframe)
      * [Load a CSV file from Amazon S3](#load-a-csv-file-from-amazon-s3)
      * [Load a CSV file from Oracle Cloud Infrastructure (OCI) Object Storage](#load-a-csv-file-from-oracle-cloud-infrastructure-oci-object-storage)
      * [Read an Oracle DB table into a DataFrame using a Wallet](#read-an-oracle-db-table-into-a-dataframe-using-a-wallet)
      * [Write a DataFrame to an Oracle DB table using a Wallet](#write-a-dataframe-to-an-oracle-db-table-using-a-wallet)
      * [Read a Postgres table into a DataFrame](#read-a-postgres-table-into-a-dataframe)
      * [Write a DataFrame to a Postgres table](#write-a-dataframe-to-a-postgres-table)
   * [Data Handling Options](#data-handling-options)
      * [Provide the schema when loading a DataFrame from CSV](#provide-the-schema-when-loading-a-dataframe-from-csv)
      * [Save a DataFrame to CSV, overwriting existing data](#save-a-dataframe-to-csv-overwriting-existing-data)
      * [Save a DataFrame to CSV with a header](#save-a-dataframe-to-csv-with-a-header)
      * [Save a DataFrame in a single CSV file](#save-a-dataframe-in-a-single-csv-file)
      * [Save DataFrame as a dynamic partitioned table](#save-dataframe-as-a-dynamic-partitioned-table)
      * [Overwrite specific partitions](#overwrite-specific-partitions)
      * [Load a CSV file with a money column into a DataFrame](#load-a-csv-file-with-a-money-column-into-a-dataframe)
   * [DataFrame Operations](#dataframe-operations)
      * [Add a new column to a DataFrame](#add-a-new-column-to-a-dataframe)
      * [Modify a DataFrame column](#modify-a-dataframe-column)
      * [Add a column with multiple conditions](#add-a-column-with-multiple-conditions)
      * [Add a constant column](#add-a-constant-column)
      * [Concatenate columns](#concatenate-columns)
      * [Drop a column](#drop-a-column)
      * [Change a column name](#change-a-column-name)
      * [Change multiple column names](#change-multiple-column-names)
      * [Convert a DataFrame column to a Python list](#convert-a-dataframe-column-to-a-python-list)
      * [Convert a scalar query to a Python value](#convert-a-scalar-query-to-a-python-value)
      * [Consume a DataFrame row-wise as Python dictionaries](#consume-a-dataframe-row-wise-as-python-dictionaries)
      * [Select particular columns from a DataFrame](#select-particular-columns-from-a-dataframe)
      * [Create an empty dataframe with a specified schema](#create-an-empty-dataframe-with-a-specified-schema)
      * [Create a constant dataframe](#create-a-constant-dataframe)
      * [Convert String to Double](#convert-string-to-double)
      * [Convert String to Integer](#convert-string-to-integer)
      * [Get the size of a DataFrame](#get-the-size-of-a-dataframe)
      * [Get a DataFrame's number of partitions](#get-a-dataframe-s-number-of-partitions)
      * [Get data types of a DataFrame's columns](#get-data-types-of-a-dataframe-s-columns)
      * [Convert an RDD to Data Frame](#convert-an-rdd-to-data-frame)
      * [Print the contents of an RDD](#print-the-contents-of-an-rdd)
      * [Print the contents of a DataFrame](#print-the-contents-of-a-dataframe)
      * [Process each row of a DataFrame](#process-each-row-of-a-dataframe)
      * [DataFrame Map example](#dataframe-map-example)
      * [DataFrame Flatmap example](#dataframe-flatmap-example)
      * [Create a custom UDF](#create-a-custom-udf)
   * [Transforming Data](#transforming-data)
      * [Extract data from a string using a regular expression](#extract-data-from-a-string-using-a-regular-expression)
      * [Fill NULL values in specific columns](#fill-null-values-in-specific-columns)
      * [Fill NULL values with column average](#fill-null-values-with-column-average)
      * [Fill NULL values with group average](#fill-null-values-with-group-average)
      * [Unpack a DataFrame's JSON column to a new DataFrame](#unpack-a-dataframe-s-json-column-to-a-new-dataframe)
      * [Query a JSON column](#query-a-json-column)
   * [Sorting and Searching](#sorting-and-searching)
      * [Filter a column using a condition](#filter-a-column-using-a-condition)
      * [Filter based on a specific column value](#filter-based-on-a-specific-column-value)
      * [Filter based on an IN list](#filter-based-on-an-in-list)
      * [Filter based on a NOT IN list](#filter-based-on-a-not-in-list)
      * [Filter values based on keys in another DataFrame](#filter-values-based-on-keys-in-another-dataframe)
      * [Get Dataframe rows that match a substring](#get-dataframe-rows-that-match-a-substring)
      * [Filter a Dataframe based on a custom substring search](#filter-a-dataframe-based-on-a-custom-substring-search)
      * [Filter based on a column's length](#filter-based-on-a-column-s-length)
      * [Multiple filter conditions](#multiple-filter-conditions)
      * [Sort DataFrame by a column](#sort-dataframe-by-a-column)
      * [Take the first N rows of a DataFrame](#take-the-first-n-rows-of-a-dataframe)
      * [Get distinct values of a column](#get-distinct-values-of-a-column)
      * [Remove duplicates](#remove-duplicates)
   * [Grouping](#grouping)
      * [count(*) on a particular column](#count-on-a-particular-column)
      * [Group and sort](#group-and-sort)
      * [Filter groups based on an aggregate value, equivalent to SQL HAVING clause](#filter-groups-based-on-an-aggregate-value-equivalent-to-sql-having-clause)
      * [Group by multiple columns](#group-by-multiple-columns)
      * [Aggregate multiple columns](#aggregate-multiple-columns)
      * [Aggregate multiple columns with custom orderings](#aggregate-multiple-columns-with-custom-orderings)
      * [Get the maximum of a column](#get-the-maximum-of-a-column)
      * [Sum a list of columns](#sum-a-list-of-columns)
      * [Sum a column](#sum-a-column)
      * [Aggregate all numeric columns](#aggregate-all-numeric-columns)
      * [Count unique after grouping](#count-unique-after-grouping)
      * [Count distinct values on all columns](#count-distinct-values-on-all-columns)
      * [Group by then filter on the count](#group-by-then-filter-on-the-count)
      * [Find the top N per row group (use N=1 for maximum)](#find-the-top-n-per-row-group-use-n-1-for-maximum)
      * [Group key/values into a list](#group-key-values-into-a-list)
      * [Compute a histogram](#compute-a-histogram)
      * [Compute global percentiles](#compute-global-percentiles)
      * [Compute percentiles within a partition](#compute-percentiles-within-a-partition)
      * [Compute percentiles after aggregating](#compute-percentiles-after-aggregating)
      * [Filter rows with values below a target percentile](#filter-rows-with-values-below-a-target-percentile)
      * [Aggregate and rollup](#aggregate-and-rollup)
      * [Aggregate and cube](#aggregate-and-cube)
   * [Joining DataFrames](#joining-dataframes)
      * [Join two DataFrames by column name](#join-two-dataframes-by-column-name)
      * [Join two DataFrames with an expression](#join-two-dataframes-with-an-expression)
      * [Multiple join conditions](#multiple-join-conditions)
      * [Various Spark join types](#various-spark-join-types)
      * [Concatenate two DataFrames](#concatenate-two-dataframes)
      * [Load multiple files into a single DataFrame](#load-multiple-files-into-a-single-dataframe)
      * [Subtract DataFrames](#subtract-dataframes)
   * [File Processing](#file-processing)
      * [Load Local File Details into a DataFrame](#load-local-file-details-into-a-dataframe)
      * [Load Files from Oracle Cloud Infrastructure into a DataFrame](#load-files-from-oracle-cloud-infrastructure-into-a-dataframe)
      * [Transform Many Images using Pillow](#transform-many-images-using-pillow)
   * [Handling Missing Data](#handling-missing-data)
      * [Filter rows with None or Null values](#filter-rows-with-none-or-null-values)
      * [Drop rows with Null values](#drop-rows-with-null-values)
      * [Count all Null or NaN values in a DataFrame](#count-all-null-or-nan-values-in-a-dataframe)
   * [Dealing with Dates](#dealing-with-dates)
      * [Convert an ISO 8601 formatted date string to date type](#convert-an-iso-8601-formatted-date-string-to-date-type)
      * [Convert a custom formatted date string to date type](#convert-a-custom-formatted-date-string-to-date-type)
      * [Get the last day of the current month](#get-the-last-day-of-the-current-month)
      * [Convert UNIX (seconds since epoch) timestamp to date](#convert-unix-seconds-since-epoch-timestamp-to-date)
      * [Load a CSV file with complex dates into a DataFrame](#load-a-csv-file-with-complex-dates-into-a-dataframe)
   * [Unstructured Analytics](#unstructured-analytics)
      * [Flatten top level text fields in a JSONl document](#flatten-top-level-text-fields-in-a-jsonl-document)
      * [Flatten top level text fields from a JSON column](#flatten-top-level-text-fields-from-a-json-column)
      * [Unnest an array of complex structures](#unnest-an-array-of-complex-structures)
   * [Pandas](#pandas)
      * [Convert Spark DataFrame to Pandas DataFrame](#convert-spark-dataframe-to-pandas-dataframe)
      * [Convert Pandas DataFrame to Spark DataFrame](#convert-pandas-dataframe-to-spark-dataframe)
      * [Convert N rows from a DataFrame to a Pandas DataFrame](#convert-n-rows-from-a-dataframe-to-a-pandas-dataframe)
      * [Grouped Aggregation with Pandas](#grouped-aggregation-with-pandas)
      * [Use a Pandas Grouped Map Function via applyInPandas](#use-a-pandas-grouped-map-function-via-applyinpandas)
   * [Data Profiling](#data-profiling)
      * [Compute the number of NULLs across all columns](#compute-the-number-of-nulls-across-all-columns)
      * [Compute average values of all numeric columns](#compute-average-values-of-all-numeric-columns)
      * [Compute minimum values of all numeric columns](#compute-minimum-values-of-all-numeric-columns)
      * [Compute maximum values of all numeric columns](#compute-maximum-values-of-all-numeric-columns)
      * [Compute median values of all numeric columns](#compute-median-values-of-all-numeric-columns)
      * [Identify Outliers in a DataFrame](#identify-outliers-in-a-dataframe)
   * [Data Management](#data-management)
      * [Save to a Delta Table](#save-to-a-delta-table)
      * [Update records in a DataFrame using Delta Tables](#update-records-in-a-dataframe-using-delta-tables)
      * [Merge into a Delta table](#merge-into-a-delta-table)
      * [Show Table Version History](#show-table-version-history)
      * [Load a Delta Table by Version ID (Time Travel Query)](#load-a-delta-table-by-version-id-time-travel-query)
      * [Load a Delta Table by Timestamp (Time Travel Query)](#load-a-delta-table-by-timestamp-time-travel-query)
      * [Compact a Delta Table](#compact-a-delta-table)
   * [Spark Streaming](#spark-streaming)
      * [Connect to Kafka using SASL PLAIN authentication](#connect-to-kafka-using-sasl-plain-authentication)
      * [Create a windowed Structured Stream over input CSV files](#create-a-windowed-structured-stream-over-input-csv-files)
      * [Create an unwindowed Structured Stream over input CSV files](#create-an-unwindowed-structured-stream-over-input-csv-files)
      * [Add the current timestamp to a DataFrame](#add-the-current-timestamp-to-a-dataframe)
      * [Call a UDF only when a threshold is reached](#call-a-udf-only-when-a-threshold-is-reached)
      * [Streaming Machine Learning](#streaming-machine-learning)
      * [Control stream processing frequency](#control-stream-processing-frequency)
   * [Time Series](#time-series)
      * [Zero fill missing values in a timeseries](#zero-fill-missing-values-in-a-timeseries)
      * [First Time an ID is Seen](#first-time-an-id-is-seen)
      * [Cumulative Sum](#cumulative-sum)
      * [Cumulative Sum in a Period](#cumulative-sum-in-a-period)
      * [Cumulative Average](#cumulative-average)
      * [Cumulative Average in a Period](#cumulative-average-in-a-period)
   * [Machine Learning](#machine-learning)
      * [Save a model](#save-a-model)
      * [Load a model and use it for predictions](#load-a-model-and-use-it-for-predictions)
      * [A basic Linear Regression model](#a-basic-linear-regression-model)
      * [A basic Random Forest Regression model](#a-basic-random-forest-regression-model)
      * [A basic Random Forest Classification model](#a-basic-random-forest-classification-model)
      * [Encode string variables before using a VectorAssembler](#encode-string-variables-before-using-a-vectorassembler)
      * [Get feature importances of a trained model](#get-feature-importances-of-a-trained-model)
      * [Automatically encode categorical variables](#automatically-encode-categorical-variables)
      * [Hyperparameter tuning](#hyperparameter-tuning)
      * [Plot Hyperparameter tuning metrics](#plot-hyperparameter-tuning-metrics)
      * [A Random Forest Classification model with Hyperparameter Tuning](#a-random-forest-classification-model-with-hyperparameter-tuning)
      * [Compute correlation matrix](#compute-correlation-matrix)
   * [Performance](#performance)
      * [Get the Spark version](#get-the-spark-version)
      * [Log messages using Spark's Log4J](#log-messages-using-spark-s-log4j)
      * [Cache a DataFrame](#cache-a-dataframe)
      * [Show the execution plan, with costs](#show-the-execution-plan-with-costs)
      * [Partition by a Column Value](#partition-by-a-column-value)
      * [Range Partition a DataFrame](#range-partition-a-dataframe)
      * [Change Number of DataFrame Partitions](#change-number-of-dataframe-partitions)
      * [Coalesce DataFrame partitions](#coalesce-dataframe-partitions)
      * [Set the number of shuffle partitions](#set-the-number-of-shuffle-partitions)
      * [Sample a subset of a DataFrame](#sample-a-subset-of-a-dataframe)
      * [Print Spark configuration properties](#print-spark-configuration-properties)
      * [Set Spark configuration properties](#set-spark-configuration-properties)
      * [Run multiple concurrent jobs in different pools](#run-multiple-concurrent-jobs-in-different-pools)
      * [Increase Spark driver/executor heap space](#increase-spark-driver-executor-heap-space)
<!--te-->
    

Accessing Data Sources
======================
Loading data stored in filesystems or databases, and saving it.

Load a DataFrame from CSV
-------------------------

```python
# See https://spark.apache.org/docs/latest/api/java/org/apache/spark/sql/DataFrameReader.html
# for a list of supported options.
df = spark.read.format("csv").option("header", True).load("data/auto-mpg.csv")
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Load a DataFrame from a Tab Separated Value (TSV) file
------------------------------------------------------

```python
# See https://spark.apache.org/docs/latest/api/java/org/apache/spark/sql/DataFrameReader.html
# for a list of supported options.
df = (
    spark.read.format("csv")
    .option("header", True)
    .option("sep", "\t")
    .load("data/auto-mpg.tsv")
)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Save a DataFrame in CSV format
------------------------------

```python
# See https://spark.apache.org/docs/latest/api/java/org/apache/spark/sql/DataFrameWriter.html
# for a list of supported options.
auto_df.write.csv("output.csv")
```


Load a DataFrame from Parquet
-----------------------------

```python
df = spark.read.format("parquet").load("data/auto-mpg.parquet")
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Save a DataFrame in Parquet format
----------------------------------

```python
auto_df.write.parquet("output.parquet")
```


Load a DataFrame from JSON Lines (jsonl) Formatted Data
-------------------------------------------------------

```python
# JSON Lines / jsonl format uses one JSON document per line.
# If you have data with mostly regular structure this is better than nesting it in an array.
# See https://jsonlines.org/
df = spark.read.json("data/weblog.jsonl")
```
```
# Code snippet result:
+----------+----------+--------+----------+----------+------+
|    client|   country| session| timestamp|       uri|  user|
+----------+----------+--------+----------+----------+------+
|{false,...|Bangladesh|55fa8213| 869196249|http://...|dde312|
|{true, ...|      Niue|2fcd4a83|1031238717|http://...|9d00b9|
|{true, ...|    Rwanda|013b996e| 628683372|http://...|1339d4|
|{false,...|   Austria|07e8a71a|1043628668|https:/...|966312|
|{false,...|    Belize|b23d05d8| 192738669|http://...|2af1e1|
|{false,...|Lao Peo...|d83dfbae|1066490444|http://...|844395|
|{false,...|French ...|e77dfaa2|1350920869|https:/...|  null|
|{false,...|Turks a...|56664269| 280986223|http://...|  null|
|{false,...|  Ethiopia|628d6059| 881914195|https:/...|8ab45a|
|{false,...|Saint K...|85f9120c|1065114708|https:/...|  null|
+----------+----------+--------+----------+----------+------+
only showing top 10 rows
```

Save a DataFrame into a Hive catalog table
------------------------------------------

```python
auto_df.write.mode("overwrite").saveAsTable("autompg")
```


Load a Hive catalog table into a DataFrame
------------------------------------------

```python
# Load the table previously saved.
df = spark.table("autompg")
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Load a CSV file from Amazon S3
------------------------------

```python
import configparser
import os

config = configparser.ConfigParser()
config.read(os.path.expanduser("~/.aws/credentials"))
access_key = config.get("default", "aws_access_key_id").replace('"', "")
secret_key = config.get("default", "aws_secret_access_key").replace('"', "")

# Requires compatible hadoop-aws and aws-java-sdk-bundle JARs.
spark.conf.set(
    "fs.s3a.aws.credentials.provider",
    "org.apache.hadoop.fs.s3a.SimpleAWSCredentialsProvider",
)
spark.conf.set("fs.s3a.access.key", access_key)
spark.conf.set("fs.s3a.secret.key", secret_key)

df = (
    spark.read.format("csv")
    .option("header", True)
    .load("s3a://cheatsheet111/auto-mpg.csv")
)
```


Load a CSV file from Oracle Cloud Infrastructure (OCI) Object Storage
---------------------------------------------------------------------

```python
import oci

oci_config = oci.config.from_file()
conf = spark.sparkContext.getConf()
conf.set("fs.oci.client.auth.tenantId", oci_config["tenancy"])
conf.set("fs.oci.client.auth.userId", oci_config["user"])
conf.set("fs.oci.client.auth.fingerprint", oci_config["fingerprint"])
conf.set("fs.oci.client.auth.pemfilepath", oci_config["key_file"])
conf.set(
    "fs.oci.client.hostname",
    "https://objectstorage.{0}.oraclecloud.com".format(oci_config["region"]),
)
PATH = "oci://<your_bucket>@<your_namespace/<your_path>"
df = spark.read.format("csv").option("header", True).load(PATH)
```


Read an Oracle DB table into a DataFrame using a Wallet
-------------------------------------------------------

```python
# Key variables you need.
# Get the tnsname from tnsnames.ora.
# Wallet path should point to an extracted wallet file.
password = "my_password"
table = "source_table"
tnsname = "my_tns_name"
user = "ADMIN"
wallet_path = "/path/to/your/wallet"

properties = {
    "driver": "oracle.jdbc.driver.OracleDriver",
    "oracle.net.tns_admin": tnsname,
    "password": password,
    "user": user,
}
url = f"jdbc:oracle:thin:@{tnsname}?TNS_ADMIN={wallet_path}"
df = spark.read.jdbc(url=url, table=table, properties=properties)
```


Write a DataFrame to an Oracle DB table using a Wallet
------------------------------------------------------

```python
# Key variables you need.
# Get the tnsname from tnsnames.ora.
# Wallet path should point to an extracted wallet file.
password = "my_password"
table = "target_table"
tnsname = "my_tns_name"
user = "ADMIN"
wallet_path = "/path/to/your/wallet"

properties = {
    "driver": "oracle.jdbc.driver.OracleDriver",
    "oracle.net.tns_admin": tnsname,
    "password": password,
    "user": user,
}
url = f"jdbc:oracle:thin:@{tnsname}?TNS_ADMIN={wallet_path}"

# Possible modes are "Append", "Overwrite", "Ignore", "Error"
df.write.jdbc(url=url, table=table, mode="Append", properties=properties)
```


Read a Postgres table into a DataFrame
--------------------------------------

```python
# You need a compatible postgresql JDBC JAR.
pg_database = os.environ.get("PGDATABASE")
pg_host = os.environ.get("PGHOST")
pg_password = os.environ.get("PGPASSWORD")
pg_user = os.environ.get("PGUSER")
table = "test"

properties = {
    "driver": "org.postgresql.Driver",
    "user": pg_user,
    "password": pg_password,
}
url = f"jdbc:postgresql://{pg_host}:5432/{pg_database}"
df = spark.read.jdbc(url=url, table=table, properties=properties)
```


Write a DataFrame to a Postgres table
-------------------------------------

```python
# You need a compatible postgresql JDBC JAR.
pg_database = os.environ.get("PGDATABASE")
pg_host = os.environ.get("PGHOST")
pg_password = os.environ.get("PGPASSWORD")
pg_user = os.environ.get("PGUSER")
table = "autompg"

properties = {
    "driver": "org.postgresql.Driver",
    "user": pg_user,
    "password": pg_password,
}
url = f"jdbc:postgresql://{pg_host}:5432/{pg_database}"
auto_df.write.jdbc(url=url, table=table, mode="Append", properties=properties)
```


Data Handling Options
=====================
Special data handling scenarios.

Provide the schema when loading a DataFrame from CSV
----------------------------------------------------

```python
# See https://spark.apache.org/docs/latest/api/python/_modules/pyspark/sql/types.html
# for a list of types.
from pyspark.sql.types import (
    DoubleType,
    IntegerType,
    StringType,
    StructField,
    StructType,
)

schema = StructType(
    [
        StructField("mpg", DoubleType(), True),
        StructField("cylinders", IntegerType(), True),
        StructField("displacement", DoubleType(), True),
        StructField("horsepower", DoubleType(), True),
        StructField("weight", DoubleType(), True),
        StructField("acceleration", DoubleType(), True),
        StructField("modelyear", IntegerType(), True),
        StructField("origin", IntegerType(), True),
        StructField("carname", StringType(), True),
    ]
)
df = (
    spark.read.format("csv")
    .option("header", "true")
    .schema(schema)
    .load("data/auto-mpg.csv")
)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0|3504.0|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0|3693.0|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0|3436.0|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0|3433.0|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0|3449.0|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0|4341.0|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0|4354.0|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0|4312.0|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0|4425.0|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0|3850.0|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Save a DataFrame to CSV, overwriting existing data
--------------------------------------------------

```python
auto_df.write.mode("overwrite").csv("output.csv")
```


Save a DataFrame to CSV with a header
-------------------------------------

```python
# See https://spark.apache.org/docs/latest/api/java/org/apache/spark/sql/DataFrameWriter.html
# for a list of supported options.
auto_df.coalesce(1).write.csv("header.csv", header="true")
```


Save a DataFrame in a single CSV file
-------------------------------------

```python
auto_df.coalesce(1).write.csv("single.csv")
```


Save DataFrame as a dynamic partitioned table
---------------------------------------------

```python
spark.conf.set("spark.sql.sources.partitionOverwriteMode", "dynamic")
auto_df.write.mode("append").partitionBy("modelyear").saveAsTable(
    "autompg_partitioned"
)
```


Overwrite specific partitions
-----------------------------

```python
spark.conf.set("spark.sql.sources.partitionOverwriteMode", "dynamic")
your_dataframe.write.mode("overwrite").insertInto("your_table")
```


Load a CSV file with a money column into a DataFrame
----------------------------------------------------

```python
from pyspark.sql.functions import udf
from pyspark.sql.types import DecimalType
from decimal import Decimal

# Load the text file.
df = (
    spark.read.format("csv")
    .option("header", True)
    .load("data/customer_spend.csv")
)

# Convert with a hardcoded custom UDF.
money_udf = udf(lambda x: Decimal(x[1:].replace(",", "")), DecimalType(8, 4))
money1 = df.withColumn("spend_dollars", money_udf(df.spend_dollars))

# Convert with the money_parser library (much safer).
from money_parser import price_str

money_convert = udf(
    lambda x: Decimal(price_str(x)) if x is not None else None,
    DecimalType(8, 4),
)
df = df.withColumn("spend_dollars", money_convert(df.spend_dollars))
```
```
# Code snippet result:
+----------+-----------+-------------+
|      date|customer_id|spend_dollars|
+----------+-----------+-------------+
|2020-01-31|          0|       0.0700|
|2020-01-31|          1|       0.9800|
|2020-01-31|          2|       0.0600|
|2020-01-31|          3|       0.6500|
|2020-01-31|          4|       0.5700|
|2020-02-29|          0|       0.1000|
|2020-02-29|          2|       4.4000|
|2020-02-29|          3|       0.3900|
|2020-02-29|          4|       2.1300|
|2020-02-29|          5|       0.8200|
+----------+-----------+-------------+
only showing top 10 rows
```

DataFrame Operations
====================
Adding, removing and modifying DataFrame columns.

Add a new column to a DataFrame
-------------------------------

```python
from pyspark.sql.functions import upper, lower

df = auto_df.withColumn("upper", upper(auto_df.carname)).withColumn(
    "lower", lower(auto_df.carname)
)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+----------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|     upper|     lower|
+----+---------+------------+----------+------+------------+---------+------+----------+----------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|CHEVROL...|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|BUICK S...|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|PLYMOUT...|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|AMC REB...|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|FORD TO...|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|FORD GA...|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|CHEVROL...|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|PLYMOUT...|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|PONTIAC...|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|AMC AMB...|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+----------+----------+
only showing top 10 rows
```

Modify a DataFrame column
-------------------------

```python
from pyspark.sql.functions import col, concat, lit

df = auto_df.withColumn("modelyear", concat(lit("19"), col("modelyear")))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|     1970|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|     1970|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|     1970|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|     1970|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|     1970|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|     1970|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|     1970|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|     1970|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|     1970|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|     1970|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Add a column with multiple conditions
-------------------------------------

```python
from pyspark.sql.functions import col, when

df = auto_df.withColumn(
    "mpg_class",
    when(col("mpg") <= 20, "low")
    .when(col("mpg") <= 30, "mid")
    .when(col("mpg") <= 40, "high")
    .otherwise("very high"),
)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+---------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|mpg_class|
+----+---------+------------+----------+------+------------+---------+------+----------+---------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|      low|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|      low|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|      low|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|      low|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|      low|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|      low|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|      low|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|      low|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|      low|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|      low|
+----+---------+------------+----------+------+------------+---------+------+----------+---------+
only showing top 10 rows
```

Add a constant column
---------------------

```python
from pyspark.sql.functions import lit

df = auto_df.withColumn("one", lit(1))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+---+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|one|
+----+---------+------------+----------+------+------------+---------+------+----------+---+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|  1|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|  1|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|  1|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|  1|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|  1|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|  1|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|  1|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|  1|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|  1|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|  1|
+----+---------+------------+----------+------+------------+---------+------+----------+---+
only showing top 10 rows
```

Concatenate columns
-------------------

```python
from pyspark.sql.functions import concat, col, lit

df = auto_df.withColumn(
    "concatenated", concat(col("cylinders"), lit("_"), col("mpg"))
)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+------------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|concatenated|
+----+---------+------------+----------+------+------------+---------+------+----------+------------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|      8_18.0|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|      8_15.0|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|      8_18.0|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|      8_16.0|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|      8_17.0|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|      8_15.0|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|      8_14.0|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|      8_14.0|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|      8_14.0|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|      8_15.0|
+----+---------+------------+----------+------+------------+---------+------+----------+------------+
only showing top 10 rows
```

Drop a column
-------------

```python
df = auto_df.drop("horsepower")
```
```
# Code snippet result:
+----+---------+------------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+------+------------+---------+------+----------+
|18.0|        8|       307.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+------+------------+---------+------+----------+
only showing top 10 rows
```

Change a column name
--------------------

```python
df = auto_df.withColumnRenamed("horsepower", "horses")
```
```
# Code snippet result:
+----+---------+------------+------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horses|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+------+------+------------+---------+------+----------+
|18.0|        8|       307.0| 130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0| 165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0| 150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0| 150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0| 140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0| 198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0| 220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0| 215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0| 225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0| 190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+------+------+------------+---------+------+----------+
only showing top 10 rows
```

Change multiple column names
----------------------------

```python
df = auto_df.withColumnRenamed("horsepower", "horses").withColumnRenamed(
    "modelyear", "year"
)
```
```
# Code snippet result:
+----+---------+------------+------+------+------------+----+------+----------+
| mpg|cylinders|displacement|horses|weight|acceleration|year|origin|   carname|
+----+---------+------------+------+------+------------+----+------+----------+
|18.0|        8|       307.0| 130.0| 3504.|        12.0|  70|     1|chevrol...|
|15.0|        8|       350.0| 165.0| 3693.|        11.5|  70|     1|buick s...|
|18.0|        8|       318.0| 150.0| 3436.|        11.0|  70|     1|plymout...|
|16.0|        8|       304.0| 150.0| 3433.|        12.0|  70|     1|amc reb...|
|17.0|        8|       302.0| 140.0| 3449.|        10.5|  70|     1|ford to...|
|15.0|        8|       429.0| 198.0| 4341.|        10.0|  70|     1|ford ga...|
|14.0|        8|       454.0| 220.0| 4354.|         9.0|  70|     1|chevrol...|
|14.0|        8|       440.0| 215.0| 4312.|         8.5|  70|     1|plymout...|
|14.0|        8|       455.0| 225.0| 4425.|        10.0|  70|     1|pontiac...|
|15.0|        8|       390.0| 190.0| 3850.|         8.5|  70|     1|amc amb...|
+----+---------+------------+------+------+------------+----+------+----------+
only showing top 10 rows
```

Convert a DataFrame column to a Python list
-------------------------------------------

```python
names = auto_df.select("carname").rdd.flatMap(lambda x: x).collect()
print(str(names[:10]))
```
```
# Code snippet result:
['chevrolet chevelle malibu', 'buick skylark 320', 'plymouth satellite', 'amc rebel sst', 'ford torino', 'ford galaxie 500', 'chevrolet impala', 'plymouth fury iii', 'pontiac catalina', 'amc ambassador dpl']
```

Convert a scalar query to a Python value
----------------------------------------

```python
average = auto_df.agg(dict(mpg="avg")).first()[0]
print(str(average))
```
```
# Code snippet result:
23.514572864321615
```

Consume a DataFrame row-wise as Python dictionaries
---------------------------------------------------

```python
first_three = auto_df.limit(3)
for row in first_three.collect():
    my_dict = row.asDict()
    print(my_dict)
```
```
# Code snippet result:

{'mpg': '18.0', 'cylinders': '8', 'displacement': '307.0', 'horsepower': '130.0', 'weight': '3504.', 'acceleration': '12.0', 'modelyear': '70', 'origin': '1', 'carname': 'chevrolet chevelle malibu'}
{'mpg': '15.0', 'cylinders': '8', 'displacement': '350.0', 'horsepower': '165.0', 'weight': '3693.', 'acceleration': '11.5', 'modelyear': '70', 'origin': '1', 'carname': 'buick skylark 320'}
{'mpg': '18.0', 'cylinders': '8', 'displacement': '318.0', 'horsepower': '150.0', 'weight': '3436.', 'acceleration': '11.0', 'modelyear': '70', 'origin': '1', 'carname': 'plymouth satellite'}
```

Select particular columns from a DataFrame
------------------------------------------

```python
df = auto_df.select(["mpg", "cylinders", "displacement"])
```
```
# Code snippet result:
+----+---------+------------+
| mpg|cylinders|displacement|
+----+---------+------------+
|18.0|        8|       307.0|
|15.0|        8|       350.0|
|18.0|        8|       318.0|
|16.0|        8|       304.0|
|17.0|        8|       302.0|
|15.0|        8|       429.0|
|14.0|        8|       454.0|
|14.0|        8|       440.0|
|14.0|        8|       455.0|
|15.0|        8|       390.0|
+----+---------+------------+
only showing top 10 rows
```

Create an empty dataframe with a specified schema
-------------------------------------------------

```python
from pyspark.sql.types import StructField, StructType, LongType, StringType

schema = StructType(
    [
        StructField("my_id", LongType(), True),
        StructField("my_string", StringType(), True),
    ]
)
df = spark.createDataFrame([], schema)
```
```
# Code snippet result:
+-----+---------+
|my_id|my_string|
+-----+---------+
+-----+---------+
```

Create a constant dataframe
---------------------------

```python
import datetime
from pyspark.sql.types import (
    StructField,
    StructType,
    LongType,
    StringType,
    TimestampType,
)

schema = StructType(
    [
        StructField("my_id", LongType(), True),
        StructField("my_string", StringType(), True),
        StructField("my_timestamp", TimestampType(), True),
    ]
)
df = spark.createDataFrame(
    [
        (1, "foo", datetime.datetime.strptime("2021-01-01", "%Y-%m-%d")),
        (2, "bar", datetime.datetime.strptime("2021-01-02", "%Y-%m-%d")),
    ],
    schema,
)
```
```
# Code snippet result:
+-----+---------+------------+
|my_id|my_string|my_timestamp|
+-----+---------+------------+
|    1|      foo|  2021-01...|
|    2|      bar|  2021-01...|
+-----+---------+------------+
```

Convert String to Double
------------------------

```python
from pyspark.sql.functions import col

df = auto_df.withColumn("horsepower", col("horsepower").cast("double"))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Convert String to Integer
-------------------------

```python
from pyspark.sql.functions import col

df = auto_df.withColumn("horsepower", col("horsepower").cast("int"))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|       130| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|       165| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|       150| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|       150| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|       140| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|       198| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|       220| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|       215| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|       225| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|       190| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Get the size of a DataFrame
---------------------------

```python
print("{} rows".format(auto_df.count()))
print("{} columns".format(len(auto_df.columns)))
```
```
# Code snippet result:
398 rows
9 columns
```

Get a DataFrame's number of partitions
--------------------------------------

```python
print("{} partition(s)".format(auto_df.rdd.getNumPartitions()))
```
```
# Code snippet result:
1 partition(s)
```

Get data types of a DataFrame's columns
---------------------------------------

```python
print(auto_df.dtypes)
```
```
# Code snippet result:
[('mpg', 'string'), ('cylinders', 'string'), ('displacement', 'string'), ('horsepower', 'string'), ('weight', 'string'), ('acceleration', 'string'), ('modelyear', 'string'), ('origin', 'string'), ('carname', 'string')]
```

Convert an RDD to Data Frame
----------------------------

```python
from pyspark.sql import Row

# First, get the RDD from the DataFrame.
rdd = auto_df.rdd

# This converts it back to an RDD with no changes.
df = rdd.map(lambda x: Row(**x.asDict())).toDF()

# This changes the rows before creating the DataFrame.
df = rdd.map(
    lambda x: Row(**{k: v * 2 for (k, v) in x.asDict().items()})
).toDF()
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|36.0|     16.0|       614.0|     260.0|7008.0|        24.0|      140|     2|chevrol...|
|30.0|     16.0|       700.0|     330.0|7386.0|        23.0|      140|     2|buick s...|
|36.0|     16.0|       636.0|     300.0|6872.0|        22.0|      140|     2|plymout...|
|32.0|     16.0|       608.0|     300.0|6866.0|        24.0|      140|     2|amc reb...|
|34.0|     16.0|       604.0|     280.0|6898.0|        21.0|      140|     2|ford to...|
|30.0|     16.0|       858.0|     396.0|8682.0|        20.0|      140|     2|ford ga...|
|28.0|     16.0|       908.0|     440.0|8708.0|        18.0|      140|     2|chevrol...|
|28.0|     16.0|       880.0|     430.0|8624.0|        17.0|      140|     2|plymout...|
|28.0|     16.0|       910.0|     450.0|8850.0|        20.0|      140|     2|pontiac...|
|30.0|     16.0|       780.0|     380.0|7700.0|        17.0|      140|     2|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Print the contents of an RDD
----------------------------

```python
rdd = auto_df.rdd
print(rdd.take(10))
```
```
# Code snippet result:
[Row(mpg='18.0', cylinders='8', displacement='307.0', horsepower='130.0', weight='3504.', acceleration='12.0', modelyear='70', origin='1', carname='chevrolet chevelle malibu'), Row(mpg='15.0', cylinders='8', displacement='350.0', horsepower='165.0', weight='3693.', acceleration='11.5', modelyear='70', origin='1', carname='buick skylark 320'), Row(mpg='18.0', cylinders='8', displacement='318.0', horsepower='150.0', weight='3436.', acceleration='11.0', modelyear='70', origin='1', carname='plymouth satellite'), Row(mpg='16.0', cylinders='8', displacement='304.0', horsepower='150.0', weight='3433.', acceleration='12.0', modelyear='70', origin='1', carname='amc rebel sst'), Row(mpg='17.0', cylinders='8', displacement='302.0', horsepower='140.0', weight='3449.', acceleration='10.5', modelyear='70', origin='1', carname='ford torino'), Row(mpg='15.0', cylinders='8', displacement='429.0', horsepower='198.0', weight='4341.', acceleration='10.0', modelyear='70', origin='1', carname='ford galaxie 500'), Row(mpg='14.0', cylinders='8', displacement='454.0', horsepower='220.0', weight='4354.', acceleration='9.0', modelyear='70', origin='1', carname='chevrolet impala'), Row(mpg='14.0', cylinders='8', displacement='440.0', horsepower='215.0', weight='4312.', acceleration='8.5', modelyear='70', origin='1', carname='plymouth fury iii'), Row(mpg='14.0', cylinders='8', displacement='455.0', horsepower='225.0', weight='4425.', acceleration='10.0', modelyear='70', origin='1', carname='pontiac catalina'), Row(mpg='15.0', cylinders='8', displacement='390.0', horsepower='190.0', weight='3850.', acceleration='8.5', modelyear='70', origin='1', carname='amc ambassador dpl')]
```

Print the contents of a DataFrame
---------------------------------

```python
auto_df.show(10)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Process each row of a DataFrame
-------------------------------

```python
import os

def foreach_function(row):
    if row.horsepower is not None:
        os.system("echo " + row.horsepower)

auto_df.foreach(foreach_function)
```


DataFrame Map example
---------------------

```python
def map_function(row):
    if row.horsepower is not None:
        return [float(row.horsepower) * 10]
    else:
        return [None]

df = auto_df.rdd.map(map_function).toDF()
```
```
# Code snippet result:
+------+
|    _1|
+------+
|1300.0|
|1650.0|
|1500.0|
|1500.0|
|1400.0|
|1980.0|
|2200.0|
|2150.0|
|2250.0|
|1900.0|
+------+
only showing top 10 rows
```

DataFrame Flatmap example
-------------------------

```python
from pyspark.sql.types import Row

def flatmap_function(row):
    if row.cylinders is not None:
        return list(range(int(row.cylinders)))
    else:
        return [None]

rdd = auto_df.rdd.flatMap(flatmap_function)
row = Row("val")
df = rdd.map(row).toDF()
```
```
# Code snippet result:
+---+
|val|
+---+
|  0|
|  1|
|  2|
|  3|
|  4|
|  5|
|  6|
|  7|
|  0|
|  1|
+---+
only showing top 10 rows
```

Create a custom UDF
-------------------

```python
from pyspark.sql.functions import col, udf
from pyspark.sql.types import StringType

first_word_udf = udf(lambda x: x.split()[0], StringType())
df = auto_df.withColumn("manufacturer", first_word_udf(col("carname")))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+------------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|manufacturer|
+----+---------+------------+----------+------+------------+---------+------+----------+------------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|   chevrolet|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|       buick|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|    plymouth|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|         amc|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|        ford|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|        ford|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|   chevrolet|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|    plymouth|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|     pontiac|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|         amc|
+----+---------+------------+----------+------+------------+---------+------+----------+------------+
only showing top 10 rows
```

Transforming Data
=================
Data conversions and other modifications.

Extract data from a string using a regular expression
-----------------------------------------------------

```python
from pyspark.sql.functions import col, regexp_extract

group = 0
df = (
    auto_df.withColumn(
        "identifier", regexp_extract(col("carname"), "(\S?\d+)", group)
    )
    .drop("acceleration")
    .drop("cylinders")
    .drop("displacement")
    .drop("modelyear")
    .drop("mpg")
    .drop("origin")
    .drop("horsepower")
    .drop("weight")
)
```
```
# Code snippet result:
+----------+----------+
|   carname|identifier|
+----------+----------+
|chevrol...|          |
|buick s...|       320|
|plymout...|          |
|amc reb...|          |
|ford to...|          |
|ford ga...|       500|
|chevrol...|          |
|plymout...|          |
|pontiac...|          |
|amc amb...|          |
+----------+----------+
only showing top 10 rows
```

Fill NULL values in specific columns
------------------------------------

```python
df = auto_df.fillna({"horsepower": 0})
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Fill NULL values with column average
------------------------------------

```python
from pyspark.sql.functions import avg

df = auto_df.fillna({"horsepower": auto_df.agg(avg("horsepower")).first()[0]})
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Fill NULL values with group average
-----------------------------------

```python
from pyspark.sql.functions import coalesce

unmodified_columns = auto_df.columns
unmodified_columns.remove("horsepower")
manufacturer_avg = auto_df.groupBy("cylinders").agg({"horsepower": "avg"})
df = auto_df.join(manufacturer_avg, "cylinders").select(
    *unmodified_columns,
    coalesce("horsepower", "avg(horsepower)").alias("horsepower"),
)
```
```
# Code snippet result:
+----+---------+------------+------+------------+---------+------+----------+----------+
| mpg|cylinders|displacement|weight|acceleration|modelyear|origin|   carname|horsepower|
+----+---------+------------+------+------------+---------+------+----------+----------+
|18.0|        8|       307.0| 3504.|        12.0|       70|     1|chevrol...|     130.0|
|15.0|        8|       350.0| 3693.|        11.5|       70|     1|buick s...|     165.0|
|18.0|        8|       318.0| 3436.|        11.0|       70|     1|plymout...|     150.0|
|16.0|        8|       304.0| 3433.|        12.0|       70|     1|amc reb...|     150.0|
|17.0|        8|       302.0| 3449.|        10.5|       70|     1|ford to...|     140.0|
|15.0|        8|       429.0| 4341.|        10.0|       70|     1|ford ga...|     198.0|
|14.0|        8|       454.0| 4354.|         9.0|       70|     1|chevrol...|     220.0|
|14.0|        8|       440.0| 4312.|         8.5|       70|     1|plymout...|     215.0|
|14.0|        8|       455.0| 4425.|        10.0|       70|     1|pontiac...|     225.0|
|15.0|        8|       390.0| 3850.|         8.5|       70|     1|amc amb...|     190.0|
+----+---------+------------+------+------------+---------+------+----------+----------+
only showing top 10 rows
```

Unpack a DataFrame's JSON column to a new DataFrame
---------------------------------------------------

```python
from pyspark.sql.functions import col, json_tuple

source = spark.sparkContext.parallelize(
    [["1", '{ "a" : 10, "b" : 11 }'], ["2", '{ "a" : 20, "b" : 21 }']]
).toDF(["id", "json"])
df = source.select("id", json_tuple(col("json"), "a", "b"))
```
```
# Code snippet result:
+---+---+---+
| id| c0| c1|
+---+---+---+
|  1| 10| 11|
|  2| 20| 21|
+---+---+---+
```

Query a JSON column
-------------------

```python
from pyspark.sql.functions import col, json_tuple

source = spark.sparkContext.parallelize(
    [["1", '{ "a" : 10, "b" : 11 }'], ["2", '{ "a" : 20, "b" : 21 }']]
).toDF(["id", "json"])
df = (
    source.select("id", json_tuple(col("json"), "a", "b"))
    .withColumnRenamed("c0", "a")
    .withColumnRenamed("c1", "b")
    .where(col("b") > 15)
)
```
```
# Code snippet result:
+---+---+---+
| id|  a|  b|
+---+---+---+
|  2| 20| 21|
+---+---+---+
```

Sorting and Searching
=====================
Filtering, sorting, removing duplicates and more.

Filter a column using a condition
---------------------------------

```python
from pyspark.sql.functions import col

df = auto_df.filter(col("mpg") > "30")
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
| 9.0|        8|       304.0|     193.0| 4732.|        18.5|       70|     1|  hi 1200d|
|30.0|        4|       79.00|     70.00| 2074.|        19.5|       71|     2|peugeot...|
|30.0|        4|       88.00|     76.00| 2065.|        14.5|       71|     2| fiat 124b|
|31.0|        4|       71.00|     65.00| 1773.|        19.0|       71|     3|toyota ...|
|35.0|        4|       72.00|     69.00| 1613.|        18.0|       71|     3|datsun ...|
|31.0|        4|       79.00|     67.00| 1950.|        19.0|       74|     3|datsun ...|
|32.0|        4|       71.00|     65.00| 1836.|        21.0|       74|     3|toyota ...|
|31.0|        4|       76.00|     52.00| 1649.|        16.5|       74|     3|toyota ...|
|32.0|        4|       83.00|     61.00| 2003.|        19.0|       74|     3|datsun 710|
|31.0|        4|       79.00|     67.00| 2000.|        16.0|       74|     2| fiat x1.9|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Filter based on a specific column value
---------------------------------------

```python
from pyspark.sql.functions import col

df = auto_df.where(col("cylinders") == "8")
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Filter based on an IN list
--------------------------

```python
from pyspark.sql.functions import col

df = auto_df.where(col("cylinders").isin(["4", "6"]))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|24.0|        4|       113.0|     95.00| 2372.|        15.0|       70|     3|toyota ...|
|22.0|        6|       198.0|     95.00| 2833.|        15.5|       70|     1|plymout...|
|18.0|        6|       199.0|     97.00| 2774.|        15.5|       70|     1|amc hornet|
|21.0|        6|       200.0|     85.00| 2587.|        16.0|       70|     1|ford ma...|
|27.0|        4|       97.00|     88.00| 2130.|        14.5|       70|     3|datsun ...|
|26.0|        4|       97.00|     46.00| 1835.|        20.5|       70|     2|volkswa...|
|25.0|        4|       110.0|     87.00| 2672.|        17.5|       70|     2|peugeot...|
|24.0|        4|       107.0|     90.00| 2430.|        14.5|       70|     2|audi 10...|
|25.0|        4|       104.0|     95.00| 2375.|        17.5|       70|     2|  saab 99e|
|26.0|        4|       121.0|     113.0| 2234.|        12.5|       70|     2|  bmw 2002|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Filter based on a NOT IN list
-----------------------------

```python
from pyspark.sql.functions import col

df = auto_df.where(~col("cylinders").isin(["4", "6"]))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Filter values based on keys in another DataFrame
------------------------------------------------

```python
from pyspark.sql.functions import col

# Our DataFrame of keys to exclude.
exclude_keys = auto_df.select(
    (col("modelyear") + 1).alias("adjusted_year")
).distinct()

# The anti join returns only keys with no matches.
filtered = auto_df.join(
    exclude_keys,
    how="left_anti",
    on=auto_df.modelyear == exclude_keys.adjusted_year,
)

# Alternatively we can register a temporary table and use a SQL expression.
exclude_keys.registerTempTable("exclude_keys")
df = auto_df.filter(
    "modelyear not in ( select adjusted_year from exclude_keys )"
)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|      8.0|       307.0|     130.0|3504.0|        12.0|       70|     1|chevrol...|
|15.0|      8.0|       350.0|     165.0|3693.0|        11.5|       70|     1|buick s...|
|18.0|      8.0|       318.0|     150.0|3436.0|        11.0|       70|     1|plymout...|
|16.0|      8.0|       304.0|     150.0|3433.0|        12.0|       70|     1|amc reb...|
|17.0|      8.0|       302.0|     140.0|3449.0|        10.5|       70|     1|ford to...|
|15.0|      8.0|       429.0|     198.0|4341.0|        10.0|       70|     1|ford ga...|
|14.0|      8.0|       454.0|     220.0|4354.0|         9.0|       70|     1|chevrol...|
|14.0|      8.0|       440.0|     215.0|4312.0|         8.5|       70|     1|plymout...|
|14.0|      8.0|       455.0|     225.0|4425.0|        10.0|       70|     1|pontiac...|
|15.0|      8.0|       390.0|     190.0|3850.0|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Get Dataframe rows that match a substring
-----------------------------------------

```python
df = auto_df.where(auto_df.carname.contains("custom"))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|16.0|        6|       225.0|     105.0| 3439.|        15.5|       71|     1|plymout...|
|13.0|        8|       350.0|     155.0| 4502.|        13.5|       72|     1|buick l...|
|14.0|        8|       318.0|     150.0| 4077.|        14.0|       72|     1|plymout...|
|15.0|        8|       318.0|     150.0| 3777.|        12.5|       73|     1|dodge c...|
|12.0|        8|       455.0|     225.0| 4951.|        11.0|       73|     1|buick e...|
|16.0|        6|       250.0|     100.0| 3278.|        18.0|       73|     1|chevrol...|
|13.0|        8|       360.0|     170.0| 4654.|        13.0|       73|     1|plymout...|
|15.0|        8|       318.0|     150.0| 3399.|        11.0|       73|     1|dodge d...|
|14.0|        8|       318.0|     150.0| 4457.|        13.5|       74|     1|dodge c...|
|19.0|        6|       225.0|     95.00| 3264.|        16.0|       75|     1|plymout...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Filter a Dataframe based on a custom substring search
-----------------------------------------------------

```python
from pyspark.sql.functions import col

df = auto_df.where(col("carname").like("%custom%"))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|16.0|        6|       225.0|     105.0| 3439.|        15.5|       71|     1|plymout...|
|13.0|        8|       350.0|     155.0| 4502.|        13.5|       72|     1|buick l...|
|14.0|        8|       318.0|     150.0| 4077.|        14.0|       72|     1|plymout...|
|15.0|        8|       318.0|     150.0| 3777.|        12.5|       73|     1|dodge c...|
|12.0|        8|       455.0|     225.0| 4951.|        11.0|       73|     1|buick e...|
|16.0|        6|       250.0|     100.0| 3278.|        18.0|       73|     1|chevrol...|
|13.0|        8|       360.0|     170.0| 4654.|        13.0|       73|     1|plymout...|
|15.0|        8|       318.0|     150.0| 3399.|        11.0|       73|     1|dodge d...|
|14.0|        8|       318.0|     150.0| 4457.|        13.5|       74|     1|dodge c...|
|19.0|        6|       225.0|     95.00| 3264.|        16.0|       75|     1|plymout...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Filter based on a column's length
---------------------------------

```python
from pyspark.sql.functions import col, length

df = auto_df.where(length(col("carname")) < 12)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|18.0|        6|       199.0|     97.00| 2774.|        15.5|       70|     1|amc hornet|
|25.0|        4|       110.0|     87.00| 2672.|        17.5|       70|     2|peugeot...|
|24.0|        4|       107.0|     90.00| 2430.|        14.5|       70|     2|audi 10...|
|25.0|        4|       104.0|     95.00| 2375.|        17.5|       70|     2|  saab 99e|
|26.0|        4|       121.0|     113.0| 2234.|        12.5|       70|     2|  bmw 2002|
|21.0|        6|       199.0|     90.00| 2648.|        15.0|       70|     1|amc gre...|
|10.0|        8|       360.0|     215.0| 4615.|        14.0|       70|     1| ford f250|
|10.0|        8|       307.0|     200.0| 4376.|        15.0|       70|     1| chevy c20|
|11.0|        8|       318.0|     210.0| 4382.|        13.5|       70|     1|dodge d200|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Multiple filter conditions
--------------------------

```python
from pyspark.sql.functions import col

# OR
df = auto_df.filter((col("mpg") > "30") | (col("acceleration") < "10"))
# AND
df = auto_df.filter((col("mpg") > "30") & (col("acceleration") < "13"))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|32.7|        6|       168.0|     132.0| 2910.|        11.4|       80|     3|datsun ...|
|30.0|        4|       135.0|     84.00| 2385.|        12.9|       81|     1|plymout...|
|32.0|        4|       135.0|     84.00| 2295.|        11.6|       82|     1|dodge r...|
+----+---------+------------+----------+------+------------+---------+------+----------+
```

Sort DataFrame by a column
--------------------------

```python
from pyspark.sql.functions import col

df = auto_df.orderBy("carname")
df = auto_df.orderBy(col("carname").desc())
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|31.9|        4|       89.00|     71.00| 1925.|        14.0|       79|     2|vw rabb...|
|44.3|        4|       90.00|     48.00| 2085.|        21.7|       80|     2|vw rabb...|
|29.0|        4|       90.00|     70.00| 1937.|        14.2|       76|     2| vw rabbit|
|41.5|        4|       98.00|     76.00| 2144.|        14.7|       80|     2| vw rabbit|
|44.0|        4|       97.00|     52.00| 2130.|        24.6|       82|     2| vw pickup|
|43.4|        4|       90.00|     48.00| 2335.|        23.7|       80|     2|vw dash...|
|30.7|        6|       145.0|     76.00| 3160.|        19.6|       81|     2|volvo d...|
|17.0|        6|       163.0|     125.0| 3140.|        13.6|       78|     2|volvo 2...|
|20.0|        4|       130.0|     102.0| 3150.|        15.7|       76|     2| volvo 245|
|22.0|        4|       121.0|     98.00| 2945.|        14.5|       75|     2|volvo 2...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Take the first N rows of a DataFrame
------------------------------------

```python
n = 10
df = auto_df.limit(n)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
```

Get distinct values of a column
-------------------------------

```python
df = auto_df.select("cylinders").distinct()
```
```
# Code snippet result:
+---------+
|cylinders|
+---------+
|        3|
|        8|
|        5|
|        6|
|        4|
+---------+
```

Remove duplicates
-----------------

```python
df = auto_df.dropDuplicates(["carname"])
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|13.0|        8|       360.0|     175.0| 3821.|        11.0|       73|     1|amc amb...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
|17.0|        8|       304.0|     150.0| 3672.|        11.5|       72|     1|amc amb...|
|19.4|        6|       232.0|     90.00| 3210.|        17.2|       78|     1|amc con...|
|18.1|        6|       258.0|     120.0| 3410.|        15.1|       78|     1|amc con...|
|23.0|        4|       151.0|      null| 3035.|        20.5|       82|     1|amc con...|
|20.2|        6|       232.0|     90.00| 3265.|        18.2|       79|     1|amc con...|
|21.0|        6|       199.0|     90.00| 2648.|        15.0|       70|     1|amc gre...|
|18.0|        6|       199.0|     97.00| 2774.|        15.5|       70|     1|amc hornet|
|18.0|        6|       258.0|     110.0| 2962.|        13.5|       71|     1|amc hor...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Grouping
========
Group DataFrame data by key to perform aggregates like counting, sums, averages, etc.

count(*) on a particular column
-------------------------------

```python
from pyspark.sql.functions import desc

# No sorting.
df = auto_df.groupBy("cylinders").count()

# With sorting.
df = auto_df.groupBy("cylinders").count().orderBy(desc("count"))
```
```
# Code snippet result:
+---------+-----+
|cylinders|count|
+---------+-----+
|        4|  204|
|        8|  103|
|        6|   84|
|        3|    4|
|        5|    3|
+---------+-----+
```

Group and sort
--------------

```python
from pyspark.sql.functions import avg, desc

df = (
    auto_df.groupBy("cylinders")
    .agg(avg("horsepower").alias("avg_horsepower"))
    .orderBy(desc("avg_horsepower"))
)
```
```
# Code snippet result:
+---------+--------------+
|cylinders|avg_horsepower|
+---------+--------------+
|        8|    158.300...|
|        6|    101.506...|
|        3|         99.25|
|        5|    82.3333...|
|        4|    78.2814...|
+---------+--------------+
```

Filter groups based on an aggregate value, equivalent to SQL HAVING clause
--------------------------------------------------------------------------

```python
from pyspark.sql.functions import col, desc

df = (
    auto_df.groupBy("cylinders")
    .count()
    .orderBy(desc("count"))
    .filter(col("count") > 100)
)
```
```
# Code snippet result:
+---------+-----+
|cylinders|count|
+---------+-----+
|        4|  204|
|        8|  103|
+---------+-----+
```

Group by multiple columns
-------------------------

```python
from pyspark.sql.functions import avg, desc

df = (
    auto_df.groupBy(["modelyear", "cylinders"])
    .agg(avg("horsepower").alias("avg_horsepower"))
    .orderBy(desc("avg_horsepower"))
)
```
```
# Code snippet result:
+---------+---------+--------------+
|modelyear|cylinders|avg_horsepower|
+---------+---------+--------------+
|       70|        8|    183.666...|
|       73|        8|         170.0|
|       71|        8|    166.857...|
|       72|        8|    159.692...|
|       77|        8|       152.375|
|       76|        8|    146.333...|
|       74|        8|         146.0|
|       75|        8|         142.0|
|       78|        8|         135.5|
|       79|        8|         131.9|
+---------+---------+--------------+
only showing top 10 rows
```

Aggregate multiple columns
--------------------------

```python
expressions = dict(horsepower="avg", weight="max", displacement="max")
df = auto_df.groupBy("modelyear").agg(expressions)
```
```
# Code snippet result:
+---------+---------------+-----------+-----------------+
|modelyear|avg(horsepower)|max(weight)|max(displacement)|
+---------+---------------+-----------+-----------------+
|       70|     147.827...|      4732.|            97.00|
|       71|     107.037...|      5140.|            98.00|
|       72|     120.178...|      4633.|            98.00|
|       73|        130.475|      4997.|            98.00|
|       74|     94.2307...|      4699.|            98.00|
|       75|     101.066...|      4668.|            97.00|
|       76|     101.117...|      4380.|            98.00|
|       77|     105.071...|      4335.|            98.00|
|       78|     99.6944...|      4080.|            98.00|
|       79|     101.206...|      4360.|            98.00|
+---------+---------------+-----------+-----------------+
only showing top 10 rows
```

Aggregate multiple columns with custom orderings
------------------------------------------------

```python
from pyspark.sql.functions import asc, desc_nulls_last

expressions = dict(horsepower="avg", weight="max", displacement="max")
orderings = [
    desc_nulls_last("max(displacement)"),
    desc_nulls_last("avg(horsepower)"),
    asc("max(weight)"),
]
df = auto_df.groupBy("modelyear").agg(expressions).orderBy(*orderings)
```
```
# Code snippet result:
+---------+---------------+-----------+-----------------+
|modelyear|avg(horsepower)|max(weight)|max(displacement)|
+---------+---------------+-----------+-----------------+
|       73|        130.475|      4997.|            98.00|
|       72|     120.178...|      4633.|            98.00|
|       71|     107.037...|      5140.|            98.00|
|       77|     105.071...|      4335.|            98.00|
|       79|     101.206...|      4360.|            98.00|
|       76|     101.117...|      4380.|            98.00|
|       78|     99.6944...|      4080.|            98.00|
|       74|     94.2307...|      4699.|            98.00|
|       82|     81.4666...|      3035.|            98.00|
|       81|     81.0357...|      3725.|            98.00|
+---------+---------------+-----------+-----------------+
only showing top 10 rows
```

Get the maximum of a column
---------------------------

```python
from pyspark.sql.functions import col, max

df = auto_df.select(max(col("horsepower")).alias("max_horsepower"))
```
```
# Code snippet result:
+--------------+
|max_horsepower|
+--------------+
|         98.00|
+--------------+
```

Sum a list of columns
---------------------

```python
exprs = {x: "sum" for x in ("weight", "cylinders", "mpg")}
df = auto_df.agg(exprs)
```
```
# Code snippet result:
+----------+-----------+--------------+
|  sum(mpg)|sum(weight)|sum(cylinders)|
+----------+-----------+--------------+
|9358.80...|  1182229.0|        2171.0|
+----------+-----------+--------------+
```

Sum a column
------------

```python
from pyspark.sql.functions import sum

df = auto_df.groupBy("cylinders").agg(sum("weight").alias("total_weight"))
```
```
# Code snippet result:
+---------+------------+
|cylinders|total_weight|
+---------+------------+
|        3|      9594.0|
|        8|    423816.0|
|        5|      9310.0|
|        6|    268651.0|
|        4|    470858.0|
+---------+------------+
```

Aggregate all numeric columns
-----------------------------

```python
numerics = set(["decimal", "double", "float", "integer", "long", "short"])
exprs = {x[0]: "sum" for x in auto_df_fixed.dtypes if x[1] in numerics}
df = auto_df_fixed.agg(exprs)
```
```
# Code snippet result:
+-----------+-----------------+--------------+----------+---------------+-----------------+
|sum(weight)|sum(acceleration)|sum(cylinders)|  sum(mpg)|sum(horsepower)|sum(displacement)|
+-----------+-----------------+--------------+----------+---------------+-----------------+
|  1182229.0|       6196.09...|        2171.0|9358.80...|        40952.0|          76983.5|
+-----------+-----------------+--------------+----------+---------------+-----------------+
```

Count unique after grouping
---------------------------

```python
from pyspark.sql.functions import countDistinct

df = auto_df.groupBy("cylinders").agg(countDistinct("mpg"))
```
```
# Code snippet result:
+---------+----------+
|cylinders|count(mpg)|
+---------+----------+
|        3|         4|
|        8|        27|
|        5|         3|
|        6|        38|
|        4|        87|
+---------+----------+
```

Count distinct values on all columns
------------------------------------

```python
from pyspark.sql.functions import countDistinct

df = auto_df.agg(*(countDistinct(c) for c in auto_df.columns))
```
```
# Code snippet result:
+----------+----------------+-------------------+-----------------+-------------+-------------------+----------------+-------------+--------------+
|count(mpg)|count(cylinders)|count(displacement)|count(horsepower)|count(weight)|count(acceleration)|count(modelyear)|count(origin)|count(carname)|
+----------+----------------+-------------------+-----------------+-------------+-------------------+----------------+-------------+--------------+
|       129|               5|                 82|               93|          351|                 96|              13|            3|           305|
+----------+----------------+-------------------+-----------------+-------------+-------------------+----------------+-------------+--------------+
```

Group by then filter on the count
---------------------------------

```python
from pyspark.sql.functions import col

df = auto_df.groupBy("cylinders").count().where(col("count") > 100)
```
```
# Code snippet result:
+---------+-----+
|cylinders|count|
+---------+-----+
|        8|  103|
|        4|  204|
+---------+-----+
```

Find the top N per row group (use N=1 for maximum)
--------------------------------------------------

```python
from pyspark.sql.functions import col, row_number
from pyspark.sql.window import Window

# To get the maximum per group, set n=1.
n = 5
w = Window().partitionBy("cylinders").orderBy(col("horsepower").desc())
df = (
    auto_df.withColumn("horsepower", col("horsepower").cast("double"))
    .withColumn("rn", row_number().over(w))
    .where(col("rn") <= n)
    .select("*")
)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+---+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname| rn|
+----+---------+------------+----------+------+------------+---------+------+----------+---+
|21.5|        3|       80.00|     110.0| 2720.|        13.5|       77|     3|mazda rx-4|  1|
|23.7|        3|       70.00|     100.0| 2420.|        12.5|       80|     3|mazda r...|  2|
|19.0|        3|       70.00|      97.0| 2330.|        13.5|       72|     3|mazda r...|  3|
|18.0|        3|       70.00|      90.0| 2124.|        13.5|       73|     3| maxda rx3|  4|
|25.0|        4|       121.0|     115.0| 2671.|        13.5|       75|     2| saab 99le|  1|
|21.6|        4|       121.0|     115.0| 2795.|        15.7|       78|     2|saab 99gle|  2|
|26.0|        4|       121.0|     113.0| 2234.|        12.5|       70|     2|  bmw 2002|  3|
|18.0|        4|       121.0|     112.0| 2933.|        14.5|       72|     2|volvo 1...|  4|
|19.0|        4|       121.0|     112.0| 2868.|        15.5|       73|     2|volvo 1...|  5|
|20.3|        5|       131.0|     103.0| 2830.|        15.9|       78|     2| audi 5000|  1|
+----+---------+------------+----------+------+------------+---------+------+----------+---+
only showing top 10 rows
```

Group key/values into a list
----------------------------

```python
from pyspark.sql.functions import col, collect_list

df = auto_df.groupBy("cylinders").agg(
    collect_list(col("carname")).alias("models")
)
```
```
# Code snippet result:
+---------+----------+
|cylinders|    models|
+---------+----------+
|        3|[mazda ...|
|        8|[chevro...|
|        5|[audi 5...|
|        6|[plymou...|
|        4|[toyota...|
+---------+----------+
```

Compute a histogram
-------------------

```python
from pyspark.sql.functions import col

# Target column must be numeric.
df = auto_df.withColumn("horsepower", col("horsepower").cast("double"))

# N is the number of bins.
N = 11
histogram = df.select("horsepower").rdd.flatMap(lambda x: x).histogram(N)
print(histogram)
```
```
# Code snippet result:
([46.0, 62.72727272727273, 79.45454545454545, 96.18181818181819, 112.9090909090909, 129.63636363636363, 146.36363636363637, 163.0909090909091, 179.8181818181818, 196.54545454545453, 213.27272727272725, 230.0], [23, 89, 102, 65, 17, 27, 32, 15, 9, 5, 8])
```

Compute global percentiles
--------------------------

```python
from pyspark.sql.functions import col, ntile
from pyspark.sql.window import Window

w = Window().orderBy(col("mpg").desc())
df = auto_df.withColumn("ntile4", ntile(4).over(w))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|ntile4|
+----+---------+------------+----------+------+------------+---------+------+----------+------+
|46.6|      4.0|        86.0|      65.0|2110.0|        17.9|       80|     3| mazda glc|     1|
|44.6|      4.0|        91.0|      67.0|1850.0|        13.8|       80|     3|honda c...|     1|
|44.3|      4.0|        90.0|      48.0|2085.0|        21.7|       80|     2|vw rabb...|     1|
|44.0|      4.0|        97.0|      52.0|2130.0|        24.6|       82|     2| vw pickup|     2|
|43.4|      4.0|        90.0|      48.0|2335.0|        23.7|       80|     2|vw dash...|     2|
|43.1|      4.0|        90.0|      48.0|1985.0|        21.5|       78|     2|volkswa...|     2|
|41.5|      4.0|        98.0|      76.0|2144.0|        14.7|       80|     2| vw rabbit|     3|
|40.9|      4.0|        85.0|      null|1835.0|        17.3|       80|     2|renault...|     3|
|40.8|      4.0|        85.0|      65.0|2110.0|        19.2|       80|     3|datsun 210|     3|
|39.4|      4.0|        85.0|      70.0|2070.0|        18.6|       78|     3|datsun ...|     4|
+----+---------+------------+----------+------+------------+---------+------+----------+------+
only showing top 10 rows
```

Compute percentiles within a partition
--------------------------------------

```python
from pyspark.sql.functions import col, ntile
from pyspark.sql.window import Window

w = Window().partitionBy("cylinders").orderBy(col("mpg").desc())
df = auto_df.withColumn("ntile4", ntile(4).over(w))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|ntile4|
+----+---------+------------+----------+------+------------+---------+------+----------+------+
|23.7|      3.0|        70.0|     100.0|2420.0|        12.5|       80|     3|mazda r...|     1|
|21.5|      3.0|        80.0|     110.0|2720.0|        13.5|       77|     3|mazda rx-4|     2|
|19.0|      3.0|        70.0|      97.0|2330.0|        13.5|       72|     3|mazda r...|     3|
|18.0|      3.0|        70.0|      90.0|2124.0|        13.5|       73|     3| maxda rx3|     4|
|46.6|      4.0|        86.0|      65.0|2110.0|        17.9|       80|     3| mazda glc|     1|
|44.6|      4.0|        91.0|      67.0|1850.0|        13.8|       80|     3|honda c...|     1|
|44.3|      4.0|        90.0|      48.0|2085.0|        21.7|       80|     2|vw rabb...|     1|
|44.0|      4.0|        97.0|      52.0|2130.0|        24.6|       82|     2| vw pickup|     1|
|43.4|      4.0|        90.0|      48.0|2335.0|        23.7|       80|     2|vw dash...|     1|
|43.1|      4.0|        90.0|      48.0|1985.0|        21.5|       78|     2|volkswa...|     1|
+----+---------+------------+----------+------+------------+---------+------+----------+------+
only showing top 10 rows
```

Compute percentiles after aggregating
-------------------------------------

```python
from pyspark.sql.functions import col, ntile
from pyspark.sql.window import Window

grouped = auto_df.groupBy("modelyear").count()
w = Window().orderBy(col("count").desc())
df = grouped.withColumn("ntile4", ntile(4).over(w))
```
```
# Code snippet result:
+---------+-----+------+
|modelyear|count|ntile4|
+---------+-----+------+
|       73|   40|     1|
|       78|   36|     1|
|       76|   34|     1|
|       82|   31|     2|
|       75|   30|     2|
|       81|   29|     2|
|       80|   29|     3|
|       70|   29|     3|
|       79|   29|     3|
|       72|   28|     4|
+---------+-----+------+
only showing top 10 rows
```

Filter rows with values below a target percentile
-------------------------------------------------

```python
from pyspark.sql.functions import col, lit
import pyspark.sql.functions as F

target_percentile = auto_df.agg(
    F.expr("percentile(mpg, 0.9)").alias("target_percentile")
).first()[0]
df = auto_df.filter(col("mpg") > lit(target_percentile))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|35.0|      4.0|        72.0|      69.0|1613.0|        18.0|       71|     3|datsun ...|
|36.0|      4.0|        79.0|      58.0|1825.0|        18.6|       77|     2|renault...|
|43.1|      4.0|        90.0|      48.0|1985.0|        21.5|       78|     2|volkswa...|
|36.1|      4.0|        98.0|      66.0|1800.0|        14.4|       78|     1|ford fi...|
|39.4|      4.0|        85.0|      70.0|2070.0|        18.6|       78|     3|datsun ...|
|36.1|      4.0|        91.0|      60.0|1800.0|        16.4|       78|     3|honda c...|
|35.7|      4.0|        98.0|      80.0|1915.0|        14.4|       79|     1|dodge c...|
|34.5|      4.0|       105.0|      70.0|2150.0|        14.9|       79|     1|plymout...|
|37.3|      4.0|        91.0|      69.0|2130.0|        14.7|       79|     2|fiat st...|
|41.5|      4.0|        98.0|      76.0|2144.0|        14.7|       80|     2| vw rabbit|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Aggregate and rollup
--------------------

```python
from pyspark.sql.functions import avg, col, count, desc

subset = auto_df.filter(col("modelyear") > 79)
df = (
    subset.rollup("modelyear", "cylinders")
    .agg(
        avg("horsepower").alias("avg_horsepower"),
        count("modelyear").alias("count"),
    )
    .orderBy(desc("modelyear"), desc("cylinders"))
)
```
```
# Code snippet result:
+---------+---------+--------------+-----+
|modelyear|cylinders|avg_horsepower|count|
+---------+---------+--------------+-----+
|       82|      6.0|    102.333...|    3|
|       82|      4.0|    79.1481...|   28|
|       82|     null|    81.4666...|   31|
|       81|      8.0|         105.0|    1|
|       81|      6.0|    100.714...|    7|
|       81|      4.0|         72.95|   21|
|       81|     null|    81.0357...|   29|
|       80|      6.0|         111.0|    2|
|       80|      5.0|          67.0|    1|
|       80|      4.0|    74.0434...|   25|
|       80|      3.0|         100.0|    1|
|       80|     null|    77.4814...|   29|
|     null|     null|    80.0588...|   89|
+---------+---------+--------------+-----+
```

Aggregate and cube
------------------

```python
from pyspark.sql.functions import avg, col, count, desc

subset = auto_df.filter(col("modelyear") > 79)
df = (
    subset.cube("modelyear", "cylinders")
    .agg(
        avg("horsepower").alias("avg_horsepower"),
        count("modelyear").alias("count"),
    )
    .orderBy(desc("modelyear"), desc("cylinders"))
)
```
```
# Code snippet result:
+---------+---------+--------------+-----+
|modelyear|cylinders|avg_horsepower|count|
+---------+---------+--------------+-----+
|       82|      6.0|    102.333...|    3|
|       82|      4.0|    79.1481...|   28|
|       82|     null|    81.4666...|   31|
|       81|      8.0|         105.0|    1|
|       81|      6.0|    100.714...|    7|
|       81|      4.0|         72.95|   21|
|       81|     null|    81.0357...|   29|
|       80|      6.0|         111.0|    2|
|       80|      5.0|          67.0|    1|
|       80|      4.0|    74.0434...|   25|
|       80|      3.0|         100.0|    1|
|       80|     null|    77.4814...|   29|
|     null|      8.0|         105.0|    1|
|     null|      6.0|    102.833...|   12|
|     null|      5.0|          67.0|    1|
|     null|      4.0|          75.7|   74|
|     null|      3.0|         100.0|    1|
|     null|     null|    80.0588...|   89|
+---------+---------+--------------+-----+
```

Joining DataFrames
==================
Joining and stacking DataFrames.

Join two DataFrames by column name
----------------------------------

```python
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

# Load a list of manufacturer / country pairs.
countries = (
    spark.read.format("csv")
    .option("header", True)
    .load("data/manufacturers.csv")
)

# Add a manufacturers column, to join with the manufacturers list.
first_word_udf = udf(lambda x: x.split()[0], StringType())
df = auto_df.withColumn("manufacturer", first_word_udf(auto_df.carname))

# The actual join.
df = df.join(countries, "manufacturer")
```
```
# Code snippet result:
+------------+----+---------+------------+----------+------+------------+---------+------+----------+-------+
|manufacturer| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|country|
+------------+----+---------+------------+----------+------+------------+---------+------+----------+-------+
|   chevrolet|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|     us|
|       buick|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|     us|
|    plymouth|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|     us|
|         amc|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|     us|
|        ford|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|     us|
|        ford|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|     us|
|   chevrolet|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|     us|
|    plymouth|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|     us|
|     pontiac|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|     us|
|         amc|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|     us|
+------------+----+---------+------------+----------+------+------------+---------+------+----------+-------+
only showing top 10 rows
```

Join two DataFrames with an expression
--------------------------------------

```python
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

# Load a list of manufacturer / country pairs.
countries = (
    spark.read.format("csv")
    .option("header", True)
    .load("data/manufacturers.csv")
)

# Add a manufacturers column, to join with the manufacturers list.
first_word_udf = udf(lambda x: x.split()[0], StringType())
df = auto_df.withColumn("manufacturer", first_word_udf(auto_df.carname))

# The actual join.
df = df.join(countries, df.manufacturer == countries.manufacturer)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+------------+------------+-------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|manufacturer|manufacturer|country|
+----+---------+------------+----------+------+------------+---------+------+----------+------------+------------+-------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|   chevrolet|   chevrolet|     us|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|       buick|       buick|     us|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|    plymouth|    plymouth|     us|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|         amc|         amc|     us|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|        ford|        ford|     us|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|        ford|        ford|     us|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|   chevrolet|   chevrolet|     us|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|    plymouth|    plymouth|     us|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|     pontiac|     pontiac|     us|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|         amc|         amc|     us|
+----+---------+------------+----------+------+------------+---------+------+----------+------------+------------+-------+
only showing top 10 rows
```

Multiple join conditions
------------------------

```python
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

# Load a list of manufacturer / country pairs.
countries = (
    spark.read.format("csv")
    .option("header", True)
    .load("data/manufacturers.csv")
)

# Add a manufacturers column, to join with the manufacturers list.
first_word_udf = udf(lambda x: x.split()[0], StringType())
df = auto_df.withColumn("manufacturer", first_word_udf(auto_df.carname))

# The actual join.
df = df.join(
    countries,
    (df.manufacturer == countries.manufacturer)
    | (df.mpg == countries.manufacturer),
)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+------------+------------+-------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|manufacturer|manufacturer|country|
+----+---------+------------+----------+------+------------+---------+------+----------+------------+------------+-------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|   chevrolet|   chevrolet|     us|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|       buick|       buick|     us|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|    plymouth|    plymouth|     us|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|         amc|         amc|     us|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|        ford|        ford|     us|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|        ford|        ford|     us|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|   chevrolet|   chevrolet|     us|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|    plymouth|    plymouth|     us|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|     pontiac|     pontiac|     us|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|         amc|         amc|     us|
+----+---------+------------+----------+------+------------+---------+------+----------+------------+------------+-------+
only showing top 10 rows
```

Various Spark join types
------------------------

```python
# Inner join on one column.
joined = auto_df.join(auto_df, "carname")

# Left (outer) join.
joined = auto_df.join(auto_df, "carname", "left")

# Left anti (not in) join.
joined = auto_df.join(auto_df, "carname", "left_anti")

# Right (outer) join.
joined = auto_df.join(auto_df, "carname", "right")

# Full join.
joined = auto_df.join(auto_df, "carname", "full")

# Cross join.
df = auto_df.crossJoin(auto_df)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Concatenate two DataFrames
--------------------------

```python
df1 = spark.read.format("csv").option("header", True).load("data/part1.csv")
df2 = spark.read.format("csv").option("header", True).load("data/part2.csv")
df = df1.union(df2)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Load multiple files into a single DataFrame
-------------------------------------------

```python
# Approach 1: Use a list.
df = (
    spark.read.format("csv")
    .option("header", True)
    .load(["data/part1.csv", "data/part2.csv"])
)

# Approach 2: Use a wildcard.
df = spark.read.format("csv").option("header", True).load("data/part*.csv")
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|20.0|        6|       225.0|     100.0| 3651.|        17.7|       76|     1|dodge a...|
|18.0|        6|       250.0|     78.00| 3574.|        21.0|       76|     1|ford gr...|
|18.5|        6|       250.0|     110.0| 3645.|        16.2|       76|     1|pontiac...|
|17.5|        6|       258.0|     95.00| 3193.|        17.8|       76|     1|amc pac...|
|29.5|        4|       97.00|     71.00| 1825.|        12.2|       76|     2|volkswa...|
|32.0|        4|       85.00|     70.00| 1990.|        17.0|       76|     3|datsun ...|
|28.0|        4|       97.00|     75.00| 2155.|        16.4|       76|     3|toyota ...|
|26.5|        4|       140.0|     72.00| 2565.|        13.6|       76|     1|ford pinto|
|20.0|        4|       130.0|     102.0| 3150.|        15.7|       76|     2| volvo 245|
|13.0|        8|       318.0|     150.0| 3940.|        13.2|       76|     1|plymout...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Subtract DataFrames
-------------------

```python
from pyspark.sql.functions import col

df = auto_df.subtract(auto_df.where(col("mpg") < "25"))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|33.5|        4|       85.00|     70.00| 1945.|        16.8|       77|     3|datsun ...|
|37.0|        4|       91.00|     68.00| 2025.|        18.2|       82|     3|mazda g...|
|26.0|        4|       116.0|     75.00| 2246.|        14.0|       74|     2|fiat 12...|
|31.0|        4|       112.0|     85.00| 2575.|        16.2|       82|     1|pontiac...|
|29.0|        4|       90.00|     70.00| 1937.|        14.0|       75|     2|volkswa...|
|37.2|        4|       86.00|     65.00| 2019.|        16.4|       80|     3|datsun 310|
|26.0|        4|       98.00|     90.00| 2265.|        15.5|       73|     2|fiat 12...|
|40.9|        4|       85.00|      null| 1835.|        17.3|       80|     2|renault...|
|35.1|        4|       81.00|     60.00| 1760.|        16.1|       81|     3|honda c...|
|32.8|        4|       78.00|     52.00| 1985.|        19.4|       78|     3|mazda g...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

File Processing
===============
Loading File Metadata and Processing Files.

Load Local File Details into a DataFrame
----------------------------------------

```python
from pyspark.sql.types import (
    StructField,
    StructType,
    LongType,
    StringType,
    TimestampType,
)
import datetime
import glob
import os

# Simple: Use glob and only file names.
files = [[x] for x in glob.glob("/etc/*")]
df = spark.createDataFrame(files)

# Advanced: Use os.walk and extended attributes.
target_path = "/etc"
entries = []
walker = os.walk(target_path)
for root, dirs, files in walker:
    for file in files:
        full_path = os.path.join(root, file)
        try:
            stat_info = os.stat(full_path)
            entries.append(
                [
                    file,
                    full_path,
                    stat_info.st_size,
                    datetime.datetime.fromtimestamp(stat_info.st_mtime),
                ]
            )
        except:
            pass
schema = StructType(
    [
        StructField("file", StringType(), False),
        StructField("path", StringType(), False),
        StructField("size", LongType(), False),
        StructField("mtime", TimestampType(), False),
    ]
)
df = spark.createDataFrame(entries, schema)
```
```
# Code snippet result:
+----------+----------+-----+----------+
|      file|      path| size|     mtime|
+----------+----------+-----+----------+
|  manpaths|/etc/ma...|   36|2020-01...|
| rc.common|/etc/rc...| 1560|2020-01...|
|auto_ma...|/etc/au...|  195|2020-01...|
| csh.login|/etc/cs...|  121|2020-01...|
|syslog....|/etc/sy...|  133|2021-12...|
|krb5.ke...|/etc/kr...| 1946|2021-12...|
|    nanorc|/etc/na...|   11|2020-01...|
|csh.logout|/etc/cs...|   39|2020-01...|
|aliases.db|/etc/al...|16384|2020-01...|
|bashrc_...|/etc/ba...| 9270|2020-01...|
+----------+----------+-----+----------+
only showing top 10 rows
```

Load Files from Oracle Cloud Infrastructure into a DataFrame
------------------------------------------------------------

```python
from pyspark.sql.types import (
    StructField,
    StructType,
    LongType,
    StringType,
    TimestampType,
)

# Requires an object_store_client object.
# See https://oracle-cloud-infrastructure-python-sdk.readthedocs.io/en/latest/api/object_storage/client/oci.object_storage.ObjectStorageClient.html
input_bucket = "oow_2019_dataflow_lab"
raw_inputs = object_store_client.list_objects(
    object_store_client.get_namespace().data,
    input_bucket,
    fields="size,md5,timeModified",
)
files = [
    [x.name, x.size, x.time_modified, x.md5] for x in raw_inputs.data.objects
]
schema = StructType(
    [
        StructField("name", StringType(), False),
        StructField("size", LongType(), True),
        StructField("modified", TimestampType(), True),
        StructField("md5", StringType(), True),
    ]
)
df = spark.createDataFrame(files, schema)
```
```
# Code snippet result:
+----------+--------+----------+----------+
|      name|    size|  modified|       md5|
+----------+--------+----------+----------+
|sharedc...|      58|2021-01...|TY0HU7h...|
|usercon...|32006919|2021-01...|igX2QgX...|
|usercon...|    4307|2021-01...|NYXxVUc...|
|usercon...|71183217|2021-01...|HlBkZ/l...|
|usercon...|16906538|2021-01...|K8qMDXT...|
|usercon...|    4774|2021-01...|cXnXiq3...|
|usercon...|     303|2021-01...|5wgh5PJ...|
|usercon...|    1634|2021-01...|3Nqbf6K...|
|usercon...|    2611|2021-01...|B8XLwDe...|
|usercon...| 2017366|2021-01...|XyKoSOA...|
+----------+--------+----------+----------+
only showing top 10 rows
```

Transform Many Images using Pillow
----------------------------------

```python
from PIL import Image
import glob

def resize_an_image(row):
    width, height = 128, 128
    file_name = row._1
    new_name = file_name.replace(".png", ".resized.png")
    img = Image.open(file_name)
    img = img.resize((width, height), Image.ANTIALIAS)
    img.save(new_name)

files = [[x] for x in glob.glob("data/resize_image?.png")]
df = spark.createDataFrame(files)
df.foreach(resize_an_image)
```


Handling Missing Data
=====================
Dealing with NULLs and NaNs in DataFrames.

Filter rows with None or Null values
------------------------------------

```python
from pyspark.sql.functions import col

df = auto_df.where(col("horsepower").isNull())
df = auto_df.where(col("horsepower").isNotNull())
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Drop rows with Null values
--------------------------

```python
# thresh controls the number of nulls before the row gets dropped.
# subset controls the columns to consider.
df = auto_df.na.drop(thresh=2, subset=("horsepower",))
```
```
# Code snippet result:
+---+---------+------------+----------+------+------------+---------+------+-------+
|mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|carname|
+---+---------+------------+----------+------+------------+---------+------+-------+
+---+---------+------------+----------+------+------------+---------+------+-------+
```

Count all Null or NaN values in a DataFrame
-------------------------------------------

```python
from pyspark.sql.functions import col, count, isnan, when

df = auto_df.select(
    [count(when(isnan(c), c)).alias(c) for c in auto_df.columns]
)
df = auto_df.select(
    [count(when(col(c).isNull(), c)).alias(c) for c in auto_df.columns]
)
```
```
# Code snippet result:
+---+---------+------------+----------+------+------------+---------+------+-------+
|mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|carname|
+---+---------+------------+----------+------+------------+---------+------+-------+
|  0|        0|           0|         6|     0|           0|        0|     0|      0|
+---+---------+------------+----------+------+------------+---------+------+-------+
```

Dealing with Dates
==================
Parsing and processing dates and times.

Convert an ISO 8601 formatted date string to date type
------------------------------------------------------

```python
from pyspark.sql.functions import col

df = spark.sparkContext.parallelize([["2021-01-01"], ["2022-01-01"]]).toDF(
    ["date_col"]
)
df = df.withColumn("date_col", col("date_col").cast("date"))
```
```
# Code snippet result:
+----------+
|  date_col|
+----------+
|2021-01-01|
|2022-01-01|
+----------+
```

Convert a custom formatted date string to date type
---------------------------------------------------

```python
from pyspark.sql.functions import col, to_date

df = spark.sparkContext.parallelize([["20210101"], ["20220101"]]).toDF(
    ["date_col"]
)
df = df.withColumn("date_col", to_date(col("date_col"), "yyyyddMM"))
```
```
# Code snippet result:
+----------+
|  date_col|
+----------+
|2021-01-01|
|2022-01-01|
+----------+
```

Get the last day of the current month
-------------------------------------

```python
from pyspark.sql.functions import col, last_day

df = spark.sparkContext.parallelize([["2020-01-01"], ["1712-02-10"]]).toDF(
    ["date_col"]
)
df = df.withColumn("date_col", col("date_col").cast("date")).withColumn(
    "last_day", last_day(col("date_col"))
)
```
```
# Code snippet result:
+----------+----------+
|  date_col|  last_day|
+----------+----------+
|2020-01-01|2020-01-31|
|1712-02-10|1712-02-29|
+----------+----------+
```

Convert UNIX (seconds since epoch) timestamp to date
----------------------------------------------------

```python
from pyspark.sql.functions import col, from_unixtime

df = spark.sparkContext.parallelize([["1590183026"], ["2000000000"]]).toDF(
    ["ts_col"]
)
df = df.withColumn("date_col", from_unixtime(col("ts_col")))
```
```
# Code snippet result:
+----------+----------+
|    ts_col|  date_col|
+----------+----------+
|1590183026|2020-05...|
|2000000000|2033-05...|
+----------+----------+
```

Load a CSV file with complex dates into a DataFrame
---------------------------------------------------

```python
from pyspark.sql.functions import udf
from pyspark.sql.types import TimestampType
import dateparser

# Use the dateparser module to convert many formats into timestamps.
date_convert = udf(
    lambda x: dateparser.parse(x) if x is not None else None, TimestampType()
)
df = (
    spark.read.format("csv")
    .option("header", True)
    .load("data/date_examples.csv")
)
df = df.withColumn("parsed", date_convert(df.date))
```
```
# Code snippet result:
+----------+----------+
|      date|    parsed|
+----------+----------+
|2012-01...|2012-01...|
|2012-01...|2011-12...|
|2012-01...|2012-01...|
|2012-01...|2012-01...|
|2012-01...|2011-12...|
|2012-01...|2012-01...|
|01-01-2...|2012-01...|
|01-01-2...|2011-12...|
|01-01-2...|2012-01...|
|01-01-2...|2012-01...|
+----------+----------+
only showing top 10 rows
```

Unstructured Analytics
======================
Analyzing unstructured data like JSON, XML, etc.

Flatten top level text fields in a JSONl document
-------------------------------------------------

```python
from pyspark.sql.functions import col

# Load JSONl into a DataFrame. Schema is inferred automatically.
base = spark.read.json("data/financial.jsonl")

# Extract interesting fields. Alias keeps columns readable.
target_json_fields = [
    col("symbol").alias("symbol"),
    col("quoteType.longName").alias("longName"),
    col("price.marketCap.raw").alias("marketCap"),
    col("summaryDetail.previousClose.raw").alias("previousClose"),
    col("summaryDetail.fiftyTwoWeekHigh.raw").alias("fiftyTwoWeekHigh"),
    col("summaryDetail.fiftyTwoWeekLow.raw").alias("fiftyTwoWeekLow"),
    col("summaryDetail.trailingPE.raw").alias("trailingPE"),
]
df = base.select(target_json_fields)
```
```
# Code snippet result:
+------+----------+----------+-------------+----------------+---------------+----------+
|symbol|  longName| marketCap|previousClose|fiftyTwoWeekHigh|fiftyTwoWeekLow|trailingPE|
+------+----------+----------+-------------+----------------+---------------+----------+
|  ACLS|Axcelis...| 747277888|        22.58|            31.5|          12.99| 21.616652|
|  AMSC|America...| 403171712|        14.69|            18.5|            4.4|      null|
|   ATH|Athene ...|6210204672|        33.31|           50.43|          13.37| 13.404612|
|  BLDR|Builder...|3659266048|        31.69|           34.69|            9.0| 17.721876|
|   BRC|Brady C...|2069810944|        41.27|           59.11|           33.0| 18.997612|
|  CATC|Cambrid...| 437452224|        64.43|            82.0|           44.2| 14.195144|
|  CBSH|Commerc...|6501258752|        58.54|           71.92|          45.51|  22.01284|
|  CFFI|C&F Fin...| 113067632|        29.18|           57.61|           28.0|  6.614728|
|  CFFN|Capitol...|1613282304|        11.28|           14.57|           8.75| 22.937624|
|  COHR|Coheren...|2668193024|       112.97|          178.08|          78.21|      null|
+------+----------+----------+-------------+----------------+---------------+----------+
only showing top 10 rows
```

Flatten top level text fields from a JSON column
------------------------------------------------

```python
from pyspark.sql.functions import col, from_json, schema_of_json

# quote/escape options needed when loading CSV containing JSON.
base = (
    spark.read.format("csv")
    .option("header", True)
    .option("quote", '"')
    .option("escape", '"')
    .load("data/financial.csv")
)

# Infer JSON schema from one entry in the DataFrame.
sample_json_document = base.select("financial_data").first()[0]
schema = schema_of_json(sample_json_document)

# Parse using this schema.
parsed = base.withColumn("parsed", from_json("financial_data", schema))

# Extract interesting fields.
target_json_fields = [
    col("parsed.symbol").alias("symbol"),
    col("parsed.quoteType.longName").alias("longName"),
    col("parsed.price.marketCap.raw").alias("marketCap"),
    col("parsed.summaryDetail.previousClose.raw").alias("previousClose"),
    col("parsed.summaryDetail.fiftyTwoWeekHigh.raw").alias("fiftyTwoWeekHigh"),
    col("parsed.summaryDetail.fiftyTwoWeekLow.raw").alias("fiftyTwoWeekLow"),
    col("parsed.summaryDetail.trailingPE.raw").alias("trailingPE"),
]
df = parsed.select(target_json_fields)
```
```
# Code snippet result:
+------+----------+----------+-------------+----------------+---------------+----------+
|symbol|  longName| marketCap|previousClose|fiftyTwoWeekHigh|fiftyTwoWeekLow|trailingPE|
+------+----------+----------+-------------+----------------+---------------+----------+
|  ACLS|Axcelis...| 747277888|        22.58|            31.5|          12.99| 21.616652|
|  AMSC|America...| 403171712|        14.69|            18.5|            4.4|      null|
|   ATH|Athene ...|6210204672|        33.31|           50.43|          13.37| 13.404612|
|  BLDR|Builder...|3659266048|         null|            null|           null|      null|
|   BRC|Brady C...|2069810944|         null|            null|           null|      null|
|  CATC|Cambrid...| 437452224|         null|            null|           null|      null|
|  CBSH|Commerc...|6501258752|         null|            null|           null|      null|
|  CFFI|C&F Fin...| 113067632|         null|            null|           null|      null|
|  CFFN|Capitol...|1613282304|         null|            null|           null|      null|
|  COHR|Coheren...|2668193024|       112.97|          178.08|          78.21|      null|
+------+----------+----------+-------------+----------------+---------------+----------+
only showing top 10 rows
```

Unnest an array of complex structures
-------------------------------------

```python
from pyspark.sql.functions import col, explode

base = spark.read.json("data/financial.jsonl")

# Analyze balance sheet data, which is held in an array of complex types.
target_json_fields = [
    col("symbol").alias("symbol"),
    col("balanceSheetHistoryQuarterly.balanceSheetStatements").alias(
        "balanceSheetStatements"
    ),
]
selected = base.select(target_json_fields)

# Select a few fields from the balance sheet statement data.
target_json_fields = [
    col("symbol").alias("symbol"),
    col("col.endDate.fmt").alias("endDate"),
    col("col.cash.raw").alias("cash"),
    col("col.totalAssets.raw").alias("totalAssets"),
    col("col.totalLiab.raw").alias("totalLiab"),
]

# Balance sheet data is in an array, use explode to generate one row per entry.
df = selected.select("symbol", explode("balanceSheetStatements")).select(
    target_json_fields
)
```
```
# Code snippet result:
+------+----------+----------+-----------+----------+
|symbol|   endDate|      cash|totalAssets| totalLiab|
+------+----------+----------+-----------+----------+
|  ACLS|2020-06-30| 190340000|  588564000| 143540000|
|  ACLS|2020-03-31| 174745000|  562573000| 135401000|
|  ACLS|2019-12-31| 139881000|  548094000| 128667000|
|  ACLS|2019-09-30| 155317000|  530477000| 122630000|
|  AMSC|2020-06-30|  20709000|  109670000|  40251000|
|  AMSC|2020-03-31|  24699000|  124109000|  51890000|
|  AMSC|2019-12-31|  25481000|  123491000|  46303000|
|  AMSC|2019-09-30|  52829000|  117443000|  40757000|
|   ATH|2020-06-30|6240000000| 1832410...|1676020...|
|   ATH|2020-03-31|5419000000| 1421790...|1316490...|
+------+----------+----------+-----------+----------+
only showing top 10 rows
```

Pandas
======
Using Python's Pandas library to augment Spark. Some operations require the pyarrow library.

Convert Spark DataFrame to Pandas DataFrame
-------------------------------------------

```python
pandas_df = auto_df.toPandas()
```
```
# Code snippet result:
      mpg cylinders displacement horsepower weight acceleration modelyear origin                    carname
0    18.0         8        307.0      130.0  3504.         12.0        70      1  chevrolet chevelle malibu
1    15.0         8        350.0      165.0  3693.         11.5        70      1          buick skylark 320
2    18.0         8        318.0      150.0  3436.         11.0        70      1         plymouth satellite
3    16.0         8        304.0      150.0  3433.         12.0        70      1              amc rebel sst
4    17.0         8        302.0      140.0  3449.         10.5        70      1                ford torino
..    ...       ...          ...        ...    ...          ...       ...    ...                        ...
393  27.0         4        140.0      86.00  2790.         15.6        82      1            ford mustang gl
394  44.0         4        97.00      52.00  2130.         24.6        82      2                  vw pickup
395  32.0         4        135.0      84.00  2295.         11.6        82      1              dodge rampage
396  28.0         4        120.0      79.00  2625.         18.6        82      1                ford ranger
397  31.0         4        119.0      82.00  2720.         19.4        82      1                 chevy s-10

[398 rows x 9 columns]
```

Convert Pandas DataFrame to Spark DataFrame
-------------------------------------------

```python
# This code converts everything to strings.
# If you want to preserve types, see https://gist.github.com/tonyfraser/79a255aa8a9d765bd5cf8bd13597171e
from pyspark.sql.types import StructField, StructType, StringType

schema = StructType(
    [StructField(name, StringType(), True) for name in pandas_df.columns]
)
df = spark.createDataFrame(pandas_df, schema)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Convert N rows from a DataFrame to a Pandas DataFrame
-----------------------------------------------------

```python
N = 10
pdf = auto_df.limit(N).toPandas()
```
```
# Code snippet result:
    mpg cylinders displacement horsepower weight acceleration modelyear origin                    carname
0  18.0         8        307.0      130.0  3504.         12.0        70      1  chevrolet chevelle malibu
1  15.0         8        350.0      165.0  3693.         11.5        70      1          buick skylark 320
2  18.0         8        318.0      150.0  3436.         11.0        70      1         plymouth satellite
3  16.0         8        304.0      150.0  3433.         12.0        70      1              amc rebel sst
4  17.0         8        302.0      140.0  3449.         10.5        70      1                ford torino
5  15.0         8        429.0      198.0  4341.         10.0        70      1           ford galaxie 500
6  14.0         8        454.0      220.0  4354.          9.0        70      1           chevrolet impala
7  14.0         8        440.0      215.0  4312.          8.5        70      1          plymouth fury iii
8  14.0         8        455.0      225.0  4425.         10.0        70      1           pontiac catalina
9  15.0         8        390.0      190.0  3850.          8.5        70      1         amc ambassador dpl
```

Grouped Aggregation with Pandas
-------------------------------

```python
from pyspark.sql.functions import pandas_udf
from pandas import DataFrame

@pandas_udf("double")
def mean_udaf(pdf: DataFrame) -> float:
    return pdf.mean()

df = auto_df.groupby("cylinders").agg(mean_udaf(auto_df["mpg"]))
```
```
# Code snippet result:
+---------+--------------+
|cylinders|mean_udaf(mpg)|
+---------+--------------+
|      3.0|         20.55|
|      4.0|    29.2867...|
|      5.0|    27.3666...|
|      6.0|    19.9857...|
|      8.0|    14.9631...|
+---------+--------------+
```

Use a Pandas Grouped Map Function via applyInPandas
---------------------------------------------------

```python
def rescale(pdf):
    minv = pdf.horsepower.min()
    maxv = pdf.horsepower.max() - minv
    return pdf.assign(horsepower=(pdf.horsepower - minv) / maxv * 100)

df = auto_df.groupby("cylinders").applyInPandas(rescale, auto_df.schema)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|19.0|      3.0|        70.0|      35.0|2330.0|        13.5|       72|     3|mazda r...|
|18.0|      3.0|        70.0|       0.0|2124.0|        13.5|       73|     3| maxda rx3|
|21.5|      3.0|        80.0|     100.0|2720.0|        13.5|       77|     3|mazda rx-4|
|23.7|      3.0|        70.0|      50.0|2420.0|        12.5|       80|     3|mazda r...|
|24.0|      4.0|       113.0|71.0144...|2372.0|        15.0|       70|     3|toyota ...|
|27.0|      4.0|        97.0|60.8695...|2130.0|        14.5|       70|     3|datsun ...|
|26.0|      4.0|        97.0|       0.0|1835.0|        20.5|       70|     2|volkswa...|
|25.0|      4.0|       110.0|59.4202...|2672.0|        17.5|       70|     2|peugeot...|
|24.0|      4.0|       107.0|63.7681...|2430.0|        14.5|       70|     2|audi 10...|
|25.0|      4.0|       104.0|71.0144...|2375.0|        17.5|       70|     2|  saab 99e|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Data Profiling
==============
Extracting key statistics out of a body of data.

Compute the number of NULLs across all columns
----------------------------------------------

```python
from pyspark.sql.functions import col, count, when

df = auto_df.select(
    [count(when(col(c).isNull(), c)).alias(c) for c in auto_df.columns]
)
```
```
# Code snippet result:
+---+---------+------------+----------+------+------------+---------+------+-------+
|mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|carname|
+---+---------+------------+----------+------+------------+---------+------+-------+
|  0|        0|           0|         6|     0|           0|        0|     0|      0|
+---+---------+------------+----------+------+------------+---------+------+-------+
```

Compute average values of all numeric columns
---------------------------------------------

```python
numerics = set(["decimal", "double", "float", "integer", "long", "short"])
exprs = {x[0]: "avg" for x in auto_df_fixed.dtypes if x[1] in numerics}
df = auto_df_fixed.agg(exprs)
```
```
# Code snippet result:
+-----------+-----------------+--------------+----------+---------------+-----------------+
|avg(weight)|avg(acceleration)|avg(cylinders)|  avg(mpg)|avg(horsepower)|avg(displacement)|
+-----------+-----------------+--------------+----------+---------------+-----------------+
| 2970.42...|       15.5680...|    5.45477...|23.5145...|     104.469...|       193.425...|
+-----------+-----------------+--------------+----------+---------------+-----------------+
```

Compute minimum values of all numeric columns
---------------------------------------------

```python
numerics = set(["decimal", "double", "float", "integer", "long", "short"])
exprs = {x[0]: "min" for x in auto_df_fixed.dtypes if x[1] in numerics}
df = auto_df_fixed.agg(exprs)
```
```
# Code snippet result:
+-----------+-----------------+--------------+--------+---------------+-----------------+
|min(weight)|min(acceleration)|min(cylinders)|min(mpg)|min(horsepower)|min(displacement)|
+-----------+-----------------+--------------+--------+---------------+-----------------+
|     1613.0|              8.0|           3.0|     9.0|           46.0|             68.0|
+-----------+-----------------+--------------+--------+---------------+-----------------+
```

Compute maximum values of all numeric columns
---------------------------------------------

```python
numerics = set(["decimal", "double", "float", "integer", "long", "short"])
exprs = {x[0]: "max" for x in auto_df_fixed.dtypes if x[1] in numerics}
df = auto_df_fixed.agg(exprs)
```
```
# Code snippet result:
+-----------+-----------------+--------------+--------+---------------+-----------------+
|max(weight)|max(acceleration)|max(cylinders)|max(mpg)|max(horsepower)|max(displacement)|
+-----------+-----------------+--------------+--------+---------------+-----------------+
|     5140.0|             24.8|           8.0|    46.6|          230.0|            455.0|
+-----------+-----------------+--------------+--------+---------------+-----------------+
```

Compute median values of all numeric columns
--------------------------------------------

```python
import pyspark.sql.functions as F

numerics = set(["decimal", "double", "float", "integer", "long", "short"])
aggregates = []
for name, dtype in auto_df_fixed.dtypes:
    if dtype not in numerics:
        continue
    aggregates.append(
        F.expr("percentile({}, 0.5)".format(name)).alias(
            "{}_median".format(name)
        )
    )
df = auto_df_fixed.agg(*aggregates)

```
```
# Code snippet result:
+----------+----------------+-------------------+-----------------+-------------+-------------------+
|mpg_median|cylinders_median|displacement_median|horsepower_median|weight_median|acceleration_median|
+----------+----------------+-------------------+-----------------+-------------+-------------------+
|      23.0|             4.0|              148.5|             93.5|       2803.5|               15.5|
+----------+----------------+-------------------+-----------------+-------------+-------------------+
```

Identify Outliers in a DataFrame
--------------------------------

```python
# This approach uses the Median Absolute Deviation.
# Outliers are based on variances in a single numeric column.
# Tune outlier sensitivity using z_score_threshold.
from pyspark.sql.functions import col, sqrt

target_column = "mpg"
z_score_threshold = 2

# Compute the median of the target column.
target_df = auto_df.select(target_column)
target_df.registerTempTable("target_column")
profiled = sqlContext.sql(
    f"select percentile({target_column}, 0.5) as median from target_column"
)

# Compute deviations.
deviations = target_df.crossJoin(profiled).withColumn(
    "deviation", sqrt((target_df[target_column] - profiled["median"]) ** 2)
)
deviations.registerTempTable("deviations")

# The Median Absolute Deviation
mad = sqlContext.sql("select percentile(deviation, 0.5) as mad from deviations")

# Add a modified z score to the original DataFrame.
df = (
    auto_df.crossJoin(mad)
    .crossJoin(profiled)
    .withColumn(
        "zscore",
        0.6745
        * sqrt((auto_df[target_column] - profiled["median"]) ** 2)
        / mad["mad"],
    )
)

df = df.where(col("zscore") > z_score_threshold)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+---+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|mad|median|    zscore|
+----+---------+------------+----------+------+------------+---------+------+----------+---+------+----------+
|43.1|      4.0|        90.0|      48.0|1985.0|        21.5|       78|     2|volkswa...|6.0|  23.0|2.25957...|
|41.5|      4.0|        98.0|      76.0|2144.0|        14.7|       80|     2| vw rabbit|6.0|  23.0|2.07970...|
|46.6|      4.0|        86.0|      65.0|2110.0|        17.9|       80|     3| mazda glc|6.0|  23.0|2.65303...|
|40.8|      4.0|        85.0|      65.0|2110.0|        19.2|       80|     3|datsun 210|6.0|  23.0|2.00101...|
|44.3|      4.0|        90.0|      48.0|2085.0|        21.7|       80|     2|vw rabb...|6.0|  23.0|2.39447...|
|43.4|      4.0|        90.0|      48.0|2335.0|        23.7|       80|     2|vw dash...|6.0|  23.0|    2.2933|
|44.6|      4.0|        91.0|      67.0|1850.0|        13.8|       80|     3|honda c...|6.0|  23.0|    2.4282|
|40.9|      4.0|        85.0|      null|1835.0|        17.3|       80|     2|renault...|6.0|  23.0|2.01225...|
|44.0|      4.0|        97.0|      52.0|2130.0|        24.6|       82|     2| vw pickup|6.0|  23.0|   2.36075|
+----+---------+------------+----------+------+------------+---------+------+----------+---+------+----------+
```

Data Management
===============
Upserts, updates and deletes on data.

Save to a Delta Table
---------------------

```python
auto_df.write.mode("overwrite").format("delta").saveAsTable("delta_table")
```


Update records in a DataFrame using Delta Tables
------------------------------------------------

```python
from pyspark.sql.functions import expr

output_path = "delta_tests"

# Currently you have to save/reload to convert from table to DataFrame.
auto_df.write.mode("overwrite").format("delta").save(output_path)
dt = DeltaTable.forPath(spark, output_path)

# Run a SQL update operation.
dt.update(
    condition=expr("carname like 'Volks%'"), set={"carname": expr("carname")}
)

# Convert back to a DataFrame.
df = dt.toDF()
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Merge into a Delta table
------------------------

```python
from pyspark.sql.functions import col, expr

# Save the original data.
output_path = "delta_tests"
auto_df.write.mode("overwrite").format("delta").save(output_path)

# Load data that corrects some car names.
corrected_df = (
    spark.read.format("csv")
    .option("header", True)
    .load("data/auto-mpg-fixed.csv")
)

# Merge the corrected data in.
dt = DeltaTable.forPath(spark, output_path)
ret = (
    dt.alias("original")
    .merge(
        corrected_df.alias("corrected"),
        "original.modelyear = corrected.modelyear and original.weight = corrected.weight and original.acceleration = corrected.acceleration",
    )
    .whenMatchedUpdate(
        condition=expr("original.carname <> corrected.carname"),
        set={"carname": col("corrected.carname")},
    )
    .whenNotMatchedInsertAll()
    .execute()
)

# Show select table history.
df = dt.history().select("version operation operationMetrics".split())

```
```
# Code snippet result:
+-------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|version|operation|operationMetrics                                                                                                                                                                                                                                                                     |
+-------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|2      |MERGE    |{numTargetRowsCopied -> 373, numTargetRowsDeleted -> 0, numTargetFilesAdded -> 1, executionTimeMs -> 3313, numTargetRowsInserted -> 0, scanTimeMs -> 1882, numTargetRowsUpdated -> 25, numOutputRows -> 398, numSourceRows -> 398, numTargetFilesRemoved -> 1, rewriteTimeMs -> 1428}|
|1      |WRITE    |{numFiles -> 1, numOutputRows -> 398, numOutputBytes -> 12032}                                                                                                                                                                                                                       |
|0      |WRITE    |{numFiles -> 1, numOutputRows -> 398, numOutputBytes -> 12032}                                                                                                                                                                                                                       |
+-------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

Show Table Version History
--------------------------

```python
# Load our table.
output_path = "delta_tests"
dt = DeltaTable.forPath(spark, output_path)

# Show select table history.
df = dt.history().select("version timestamp operation".split())
```
```
# Code snippet result:
+-------+----------+---------+
|version| timestamp|operation|
+-------+----------+---------+
|      2|2022-03...|    MERGE|
|      1|2022-03...|    WRITE|
|      0|2022-03...|    WRITE|
+-------+----------+---------+
```

Load a Delta Table by Version ID (Time Travel Query)
----------------------------------------------------

```python
from pyspark.sql.functions import desc

# Get versions.
output_path = "delta_tests"
dt = DeltaTable.forPath(spark, output_path)
versions = (
    dt.history().select("version timestamp".split()).orderBy(desc("version"))
)
most_recent_version = versions.first()[0]
print("Most recent version is", most_recent_version)

# Load the most recent data.
df = (
    spark.read.format("delta")
    .option("versionAsOf", most_recent_version)
    .load(output_path)
)

```
```
# Code snippet result:
Most recent version is 2
```

Load a Delta Table by Timestamp (Time Travel Query)
---------------------------------------------------

```python
from pyspark.sql.functions import desc

# Get versions.
output_path = "delta_tests"
dt = DeltaTable.forPath(spark, output_path)
versions = dt.history().select("version timestamp".split()).orderBy("timestamp")
most_recent_timestamp = versions.first()[1]
print("Most recent timestamp is", most_recent_timestamp)

# Load the oldest version by timestamp.
df = (
    spark.read.format("delta")
    .option("timestampAsOf", most_recent_timestamp)
    .load(output_path)
)

```
```
# Code snippet result:
Most recent timestamp is 2022-03-13 08:57:10.063000
```

Compact a Delta Table
---------------------

```python
output_path = "delta_tests"

# Load table.
dt = DeltaTable.forPath(spark, output_path)

# Clean up data older than the given window.
retention_window_hours = 168
dt.vacuum(retention_window_hours)

# Show the new versions.
df = dt.history().select("version timestamp".split()).orderBy("version")
```
```
# Code snippet result:
+-------+----------+
|version| timestamp|
+-------+----------+
|      0|2022-03...|
|      1|2022-03...|
|      2|2022-03...|
+-------+----------+
```

Spark Streaming
===============
Spark Streaming (Focuses on Structured Streaming).

Connect to Kafka using SASL PLAIN authentication
------------------------------------------------

```python
options = {
    "kafka.sasl.jaas.config": 'org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";',
    "kafka.sasl.mechanism": "PLAIN",
    "kafka.security.protocol": "SASL_SSL",
    "kafka.bootstrap.servers": "server:9092",
    "group.id": "my_group",
    "subscribe": "my_topic",
}
df = spark.readStream.format("kafka").options(**options).load()
```


Create a windowed Structured Stream over input CSV files
--------------------------------------------------------

```python
from pyspark.sql.functions import avg, count, current_timestamp, window
from pyspark.sql.types import (
    StructField,
    StructType,
    DoubleType,
    IntegerType,
    StringType,
)

input_location = "streaming/input"
schema = StructType(
    [
        StructField("mpg", DoubleType(), True),
        StructField("cylinders", IntegerType(), True),
        StructField("displacement", DoubleType(), True),
        StructField("horsepower", DoubleType(), True),
        StructField("weight", DoubleType(), True),
        StructField("acceleration", DoubleType(), True),
        StructField("modelyear", IntegerType(), True),
        StructField("origin", IntegerType(), True),
        StructField("carname", StringType(), True),
        StructField("manufacturer", StringType(), True),
    ]
)
df = spark.readStream.csv(path=input_location, schema=schema).withColumn(
    "timestamp", current_timestamp()
)

aggregated = (
    df.groupBy(window(df.timestamp, "1 minute"), "manufacturer")
    .agg(
        avg("horsepower").alias("avg_horsepower"),
        avg("timestamp").alias("avg_timestamp"),
        count("modelyear").alias("count"),
    )
    .coalesce(10)
)
summary = aggregated.orderBy("window", "manufacturer")
query = (
    summary.writeStream.outputMode("complete")
    .format("console")
    .option("truncate", False)
    .start()
)
query.awaitTermination()
```
```
# Code snippet result:

-------------------------------------------                                     
Batch: 0
-------------------------------------------
+------------------------------------------+------------+--------------+----------------+-----+
|window                                    |manufacturer|avg_horsepower|avg_timestamp   |count|
+------------------------------------------+------------+--------------+----------------+-----+
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|amc         |131.75        |1.616243250178E9|4    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|chevrolet   |175.0         |1.616243250178E9|4    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|datsun      |88.0          |1.616243250178E9|1    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|dodge       |190.0         |1.616243250178E9|2    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|ford        |159.5         |1.616243250178E9|4    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|plymouth    |155.0         |1.616243250178E9|4    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|pontiac     |225.0         |1.616243250178E9|1    |
+------------------------------------------+------------+--------------+----------------+-----+

-------------------------------------------                                     
Batch: 1
-------------------------------------------
+------------------------------------------+------------+------------------+--------------------+-----+
|window                                    |manufacturer|avg_horsepower    |avg_timestamp       |count|
+------------------------------------------+------------+------------------+--------------------+-----+
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|amc         |119.57142857142857|1.6162432596022856E9|7    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|chevrolet   |140.875           |1.6162432611729999E9|8    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|datsun      |81.66666666666667 |1.616243264838E9    |3    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|dodge       |186.66666666666666|1.6162432575080001E9|3    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|ford        |142.125           |1.6162432623946667E9|9    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|plymouth    |135.0             |1.6162432596022856E9|7    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|pontiac     |168.75            |1.6162432666704998E9|4    |
+------------------------------------------+------------+------------------+--------------------+-----+

-------------------------------------------                                     
Batch: 2
-------------------------------------------
+------------------------------------------+------------+------------------+--------------------+-----+
|window                                    |manufacturer|avg_horsepower    |avg_timestamp       |count|
+------------------------------------------+------------+------------------+--------------------+-----+
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|amc         |119.57142857142857|1.6162432596022856E9|7    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|chevrolet   |140.875           |1.6162432611729999E9|8    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|datsun      |81.66666666666667 |1.616243264838E9    |3    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|dodge       |186.66666666666666|1.6162432575080001E9|3    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|ford        |142.125           |1.6162432623946667E9|9    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|plymouth    |135.0             |1.6162432596022856E9|7    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|pontiac     |168.75            |1.6162432666704998E9|4    |
|{2021-03-20 05:28:00, 2021-03-20 05:29:00}|amc         |150.0             |1.616243297163E9    |2    |
|{2021-03-20 05:28:00, 2021-03-20 05:29:00}|chevrolet   |128.33333333333334|1.616243297163E9    |3    |
|{2021-03-20 05:28:00, 2021-03-20 05:29:00}|datsun      |92.0              |1.616243297163E9    |1    |
|{2021-03-20 05:28:00, 2021-03-20 05:29:00}|dodge       |80.0              |1.616243297163E9    |2    |
|{2021-03-20 05:28:00, 2021-03-20 05:29:00}|ford        |116.25            |1.616243297163E9    |4    |
|{2021-03-20 05:28:00, 2021-03-20 05:29:00}|plymouth    |150.0             |1.616243297163E9    |2    |
|{2021-03-20 05:28:00, 2021-03-20 05:29:00}|pontiac     |175.0             |1.616243297163E9    |1    |
+------------------------------------------+------------+------------------+--------------------+-----+

-------------------------------------------                                     
Batch: 3
-------------------------------------------
+------------------------------------------+------------+------------------+--------------------+-----+
|window                                    |manufacturer|avg_horsepower    |avg_timestamp       |count|
+------------------------------------------+------------+------------------+--------------------+-----+
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|amc         |119.57142857142857|1.6162432596022856E9|7    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|chevrolet   |140.875           |1.6162432611729999E9|8    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|datsun      |81.66666666666667 |1.616243264838E9    |3    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|dodge       |186.66666666666666|1.6162432575080001E9|3    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|ford        |142.125           |1.6162432623946667E9|9    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|plymouth    |135.0             |1.6162432596022856E9|7    |
|{2021-03-20 05:27:00, 2021-03-20 05:28:00}|pontiac     |168.75            |1.6162432666704998E9|4    |
|{2021-03-20 05:28:00, 2021-03-20 05:29:00}|amc         |137.5             |1.616243313837E9    |6    |
|{2021-03-20 05:28:00, 2021-03-20 05:29:00}|chevrolet   |127.44444444444444|1.6162433138370001E9|9    |
|{2021-03-20 05:28:00, 2021-03-20 05:29:00}|datsun      |93.0              |1.6162433096685E9   |2    |
|{2021-03-20 05:28:00, 2021-03-20 05:29:00}|dodge       |115.0             |1.6162433096685E9   |4    |
|{2021-03-20 05:28:00, 2021-03-20 05:29:00}|ford        |122.22222222222223|1.6162433110579998E9|9    |
|{2021-03-20 05:28:00, 2021-03-20 05:29:00}|plymouth    |136.66666666666666|1.616243313837E9    |6    |
|{2021-03-20 05:28:00, 2021-03-20 05:29:00}|pontiac     |202.5             |1.6162433096685E9   |2    |
+------------------------------------------+------------+------------------+--------------------+-----+
```

Create an unwindowed Structured Stream over input CSV files
-----------------------------------------------------------

```python
from pyspark.sql.functions import avg, count, desc
from pyspark.sql.types import (
    StructField,
    StructType,
    DoubleType,
    IntegerType,
    StringType,
)

input_location = "streaming/input"
schema = StructType(
    [
        StructField("mpg", DoubleType(), True),
        StructField("cylinders", IntegerType(), True),
        StructField("displacement", DoubleType(), True),
        StructField("horsepower", DoubleType(), True),
        StructField("weight", DoubleType(), True),
        StructField("acceleration", DoubleType(), True),
        StructField("modelyear", IntegerType(), True),
        StructField("origin", IntegerType(), True),
        StructField("carname", StringType(), True),
        StructField("manufacturer", StringType(), True),
    ]
)

df = spark.readStream.csv(path=input_location, schema=schema)
summary = (
    df.groupBy("modelyear")
    .agg(
        avg("horsepower").alias("avg_horsepower"),
        count("modelyear").alias("count"),
    )
    .orderBy(desc("modelyear"))
    .coalesce(10)
)
query = summary.writeStream.outputMode("complete").format("console").start()
query.awaitTermination()
```
```
# Code snippet result:

-------------------------------------------
Batch: 0
-------------------------------------------
+---------+------------------+-----+
|modelyear|    avg_horsepower|count|
+---------+------------------+-----+
|       70|147.82758620689654|   29|
+---------+------------------+-----+

-------------------------------------------
Batch: 1
-------------------------------------------
+---------+------------------+-----+
|modelyear|    avg_horsepower|count|
+---------+------------------+-----+
|       71|107.03703703703704|   28|
|       70|147.82758620689654|   29|
+---------+------------------+-----+

-------------------------------------------
Batch: 2
-------------------------------------------
+---------+------------------+-----+
|modelyear|    avg_horsepower|count|
+---------+------------------+-----+
|       72|120.17857142857143|   28|
|       71|107.03703703703704|   28|
|       70|147.82758620689654|   29|
+---------+------------------+-----+
```

Add the current timestamp to a DataFrame
----------------------------------------

```python
from pyspark.sql.functions import current_timestamp

df = auto_df.withColumn("timestamp", current_timestamp())
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname| timestamp|
+----+---------+------------+----------+------+------------+---------+------+----------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|2022-03...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|2022-03...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|2022-03...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|2022-03...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|2022-03...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|2022-03...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|2022-03...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|2022-03...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|2022-03...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|2022-03...|
+----+---------+------------+----------+------+------------+---------+------+----------+----------+
only showing top 10 rows
```

Call a UDF only when a threshold is reached
-------------------------------------------

```python
"""
It's common you want to call a UDF when measure hits a threshold. You want
to call the UDF when a row hits a condition and skip it otherwise. PySpark
does not support calling UDFs conditionally (or short-circuiting) as of 3.1.2.

To deal with this put a short-circuit field in the UDF and call the UDF
with the condition. If the short-circuit is true return immediately.

This example performs an action when a running average exceeds 100.
"""
from pyspark.sql.types import BooleanType
from pyspark.sql.functions import udf

@udf(returnType=BooleanType())
def myudf(short_circuit, state, value):
    if short_circuit == True:
        return True

    # Log, send an alert, etc.
    return False

df = (
    spark.readStream.format("socket")
    .option("host", "localhost")
    .option("port", "9090")
    .load()
)
parsed = (
    df.selectExpr(
        "split(value,',')[0] as state",
        "split(value,',')[1] as zipcode",
        "split(value,',')[2] as spend",
    )
    .groupBy("state")
    .agg({"spend": "avg"})
    .orderBy(desc("avg(spend)"))
)
tagged = parsed.withColumn(
    "below", myudf(col("avg(spend)") < 100, col("state"), col("avg(spend)"))
)

tagged.writeStream.outputMode("complete").format(
    "console"
).start().awaitTermination()
```


Streaming Machine Learning
--------------------------

```python
"""
MLlib pipelines can be loaded and used in streaming jobs.

When training, define a Pipeline like:
    pipeline = Pipeline(stages=[a, b, ...])

Fit it, then save the resulting model:
    pipeline_model = pipeline.fit(train_df)
    pipeline_model.write().overwrite().save("path/to/pipeline")

The pipeline model can then be used as shown below.
"""

from pyspark.ml import PipelineModel

pipeline_model = PipelineModel.load("path/to/pipeline")
df = pipeline.transform(input_df)
df.writeStream.format("console").start().awaitTermination()
```


Control stream processing frequency
-----------------------------------

```python
"""
Use the processingTime option of trigger to control how frequently microbatches
run. You can specify milliseconds or a string interval.
"""
df.writeStream.outputMode("complete").format("console").trigger(
    processingTime="10 seconds"
).start().awaitTermination()
```


Time Series
===========
Techniques for dealing with time series data.

Zero fill missing values in a timeseries
----------------------------------------

```python
from pyspark.sql.functions import coalesce, lit

# Use distinct values of customer and date from the dataset itself.
# In general it's safer to use known reference tables for IDs and dates.
df = spend_df.join(
    spend_df.select("customer_id")
    .distinct()
    .crossJoin(spend_df.select("date").distinct()),
    ["date", "customer_id"],
    "right",
).select("date", "customer_id", coalesce("spend_dollars", lit(0)))
```
```
# Code snippet result:
+----------+-----------+--------------------------+
|      date|customer_id|coalesce(spend_dollars, 0)|
+----------+-----------+--------------------------+
|2022-07-31|         31|                   16.4400|
|2020-04-30|         31|                    0.0000|
|2022-01-31|         31|                   25.1100|
|2021-09-30|         31|                    2.1400|
|2021-05-31|         31|                   34.3900|
|2022-02-28|         31|                   59.7800|
|2021-07-31|         31|                    8.2700|
|2020-07-31|         31|                    0.0000|
|2021-04-30|         31|                   20.7000|
|2021-10-31|         31|                   62.1500|
+----------+-----------+--------------------------+
only showing top 10 rows
```

First Time an ID is Seen
------------------------

```python
from pyspark.sql.functions import first
from pyspark.sql.window import Window

w = Window().partitionBy("customer_id").orderBy("date")
df = spend_df.withColumn("first_seen", first("date").over(w))
```
```
# Code snippet result:
+----------+-----------+-------------+----------+
|      date|customer_id|spend_dollars|first_seen|
+----------+-----------+-------------+----------+
|2020-01-31|          0|       0.0700|2020-01-31|
|2020-02-29|          0|       0.1000|2020-01-31|
|2020-04-30|          0|       0.0100|2020-01-31|
|2020-06-30|          0|       0.0300|2020-01-31|
|2020-07-31|          0|       0.0300|2020-01-31|
|2020-08-31|          0|       0.0500|2020-01-31|
|2020-09-30|          0|       0.0000|2020-01-31|
|2020-10-31|          0|       0.0300|2020-01-31|
|2020-11-30|          0|       0.0900|2020-01-31|
|2020-12-31|          0|       0.0600|2020-01-31|
+----------+-----------+-------------+----------+
only showing top 10 rows
```

Cumulative Sum
--------------

```python
from pyspark.sql.functions import sum
from pyspark.sql.window import Window

w = (
    Window()
    .partitionBy("customer_id")
    .orderBy("date")
    .rangeBetween(Window.unboundedPreceding, 0)
)
df = spend_df.withColumn("running_sum", sum("spend_dollars").over(w))
```
```
# Code snippet result:
+----------+-----------+-------------+-----------+
|      date|customer_id|spend_dollars|running_sum|
+----------+-----------+-------------+-----------+
|2020-01-31|          0|       0.0700|     0.0700|
|2020-02-29|          0|       0.1000|     0.1700|
|2020-04-30|          0|       0.0100|     0.1800|
|2020-06-30|          0|       0.0300|     0.2100|
|2020-07-31|          0|       0.0300|     0.2400|
|2020-08-31|          0|       0.0500|     0.2900|
|2020-09-30|          0|       0.0000|     0.2900|
|2020-10-31|          0|       0.0300|     0.3200|
|2020-11-30|          0|       0.0900|     0.4100|
|2020-12-31|          0|       0.0600|     0.4700|
+----------+-----------+-------------+-----------+
only showing top 10 rows
```

Cumulative Sum in a Period
--------------------------

```python
from pyspark.sql.functions import sum, year
from pyspark.sql.window import Window

# Add an additional partition clause for the sub-period.
w = (
    Window()
    .partitionBy(["customer_id", year("date")])
    .orderBy("date")
    .rangeBetween(Window.unboundedPreceding, 0)
)
df = spend_df.withColumn("running_sum", sum("spend_dollars").over(w))
```
```
# Code snippet result:
+----------+-----------+-------------+-----------+
|      date|customer_id|spend_dollars|running_sum|
+----------+-----------+-------------+-----------+
|2020-01-31|          0|       0.0700|     0.0700|
|2020-02-29|          0|       0.1000|     0.1700|
|2020-04-30|          0|       0.0100|     0.1800|
|2020-06-30|          0|       0.0300|     0.2100|
|2020-07-31|          0|       0.0300|     0.2400|
|2020-08-31|          0|       0.0500|     0.2900|
|2020-09-30|          0|       0.0000|     0.2900|
|2020-10-31|          0|       0.0300|     0.3200|
|2020-11-30|          0|       0.0900|     0.4100|
|2020-12-31|          0|       0.0600|     0.4700|
+----------+-----------+-------------+-----------+
only showing top 10 rows
```

Cumulative Average
------------------

```python
from pyspark.sql.functions import avg
from pyspark.sql.window import Window

w = (
    Window()
    .partitionBy("customer_id")
    .orderBy("date")
    .rangeBetween(Window.unboundedPreceding, 0)
)
df = spend_df.withColumn("running_avg", avg("spend_dollars").over(w))
```
```
# Code snippet result:
+----------+-----------+-------------+-----------+
|      date|customer_id|spend_dollars|running_avg|
+----------+-----------+-------------+-----------+
|2020-01-31|          0|       0.0700| 0.07000000|
|2020-02-29|          0|       0.1000| 0.08500000|
|2020-04-30|          0|       0.0100| 0.06000000|
|2020-06-30|          0|       0.0300| 0.05250000|
|2020-07-31|          0|       0.0300| 0.04800000|
|2020-08-31|          0|       0.0500| 0.04833333|
|2020-09-30|          0|       0.0000| 0.04142857|
|2020-10-31|          0|       0.0300| 0.04000000|
|2020-11-30|          0|       0.0900| 0.04555556|
|2020-12-31|          0|       0.0600| 0.04700000|
+----------+-----------+-------------+-----------+
only showing top 10 rows
```

Cumulative Average in a Period
------------------------------

```python
from pyspark.sql.functions import avg, year
from pyspark.sql.window import Window

# Add an additional partition clause for the sub-period.
w = (
    Window()
    .partitionBy(["customer_id", year("date")])
    .orderBy("date")
    .rangeBetween(Window.unboundedPreceding, 0)
)
df = spend_df.withColumn("running_avg", avg("spend_dollars").over(w))
```
```
# Code snippet result:
+----------+-----------+-------------+-----------+
|      date|customer_id|spend_dollars|running_avg|
+----------+-----------+-------------+-----------+
|2020-01-31|          0|       0.0700| 0.07000000|
|2020-02-29|          0|       0.1000| 0.08500000|
|2020-04-30|          0|       0.0100| 0.06000000|
|2020-06-30|          0|       0.0300| 0.05250000|
|2020-07-31|          0|       0.0300| 0.04800000|
|2020-08-31|          0|       0.0500| 0.04833333|
|2020-09-30|          0|       0.0000| 0.04142857|
|2020-10-31|          0|       0.0300| 0.04000000|
|2020-11-30|          0|       0.0900| 0.04555556|
|2020-12-31|          0|       0.0600| 0.04700000|
+----------+-----------+-------------+-----------+
only showing top 10 rows
```

Machine Learning
================
Machine Learning

Save a model
------------

```python
from pyspark.ml.feature import VectorAssembler
from pyspark.ml.regression import RandomForestRegressor

vectorAssembler = VectorAssembler(
    inputCols=[
        "cylinders",
        "displacement",
        "horsepower",
        "weight",
        "acceleration",
    ],
    outputCol="features",
    handleInvalid="skip",
)
assembled = vectorAssembler.transform(auto_df_fixed)

# Random test/train split.
train_df, test_df = assembled.randomSplit([0.7, 0.3])

# Define the model.
rf = RandomForestRegressor(
    numTrees=50,
    featuresCol="features",
    labelCol="mpg",
)

# Train the model.
rf_model = rf.fit(train_df)
rf_model.write().overwrite().save("rf_regression.model")
```


Load a model and use it for predictions
---------------------------------------

```python
from pyspark.ml.feature import VectorAssembler
from pyspark.ml.regression import RandomForestRegressionModel

# Model type and assembled features need to agree with the trained model.
rf_model = RandomForestRegressionModel.load("rf_regression.model")
vectorAssembler = VectorAssembler(
    inputCols=[
        "cylinders",
        "displacement",
        "horsepower",
        "weight",
        "acceleration",
    ],
    outputCol="features",
    handleInvalid="skip",
)
assembled = vectorAssembler.transform(auto_df_fixed)

predictions = rf_model.transform(assembled).select(
    "carname", "mpg", "prediction"
)
```
```
# Code snippet result:
+----------+----+----------+
|   carname| mpg|prediction|
+----------+----+----------+
|chevrol...|18.0|16.6118...|
|buick s...|15.0|14.2915...|
|plymout...|18.0|15.1508...|
|amc reb...|16.0|15.5965...|
|ford to...|17.0|16.4334...|
|ford ga...|15.0|13.7456...|
|chevrol...|14.0|13.4012...|
|plymout...|14.0|14.1575...|
|pontiac...|14.0|13.4012...|
|amc amb...|15.0|14.2642...|
+----------+----+----------+
only showing top 10 rows
```

A basic Linear Regression model
-------------------------------

```python
from pyspark.ml.feature import VectorAssembler
from pyspark.ml.regression import LinearRegression

vectorAssembler = VectorAssembler(
    inputCols=[
        "cylinders",
        "displacement",
        "horsepower",
        "weight",
        "acceleration",
    ],
    outputCol="features",
    handleInvalid="skip",
)
assembled = vectorAssembler.transform(auto_df_fixed)
assembled = assembled.select(["features", "mpg", "carname"])

# Random test/train split.
train_df, test_df = assembled.randomSplit([0.7, 0.3])

# Define the model.
lr = LinearRegression(
    featuresCol="features",
    labelCol="mpg",
    maxIter=10,
    regParam=0.3,
    elasticNetParam=0.8,
)

# Train the model.
lr_model = lr.fit(train_df)

# Stats for training.
print(
    "RMSE={} r2={}".format(
        lr_model.summary.rootMeanSquaredError, lr_model.summary.r2
    )
)

# Make predictions.
df = lr_model.transform(test_df)
```
```
# Code snippet result:
+----------+----+----------+----------+
|  features| mpg|   carname|prediction|
+----------+----+----------+----------+
|[4.0,79...|30.0|peugeot...|30.0847...|
|[4.0,85...|29.0|chevrol...|31.3284...|
|[4.0,85...|33.5|datsun ...|30.5980...|
|[4.0,85...|32.0|datsun ...|30.4062...|
|[4.0,85...|39.4|datsun ...|30.0653...|
|[4.0,90...|43.1|volkswa...|31.7586...|
|[4.0,90...|29.0| vw rabbit|30.6016...|
|[4.0,91...|36.1|honda c...|31.7983...|
|[4.0,91...|37.3|fiat st...|29.8349...|
|[4.0,91...|26.0|plymout...|30.5188...|
+----------+----+----------+----------+
only showing top 10 rows
```

A basic Random Forest Regression model
--------------------------------------

```python
from pyspark.ml.feature import VectorAssembler
from pyspark.ml.regression import RandomForestRegressor
from pyspark.ml.evaluation import RegressionEvaluator

vectorAssembler = VectorAssembler(
    inputCols=[
        "cylinders",
        "displacement",
        "horsepower",
        "weight",
        "acceleration",
    ],
    outputCol="features",
    handleInvalid="skip",
)
assembled = vectorAssembler.transform(auto_df_fixed)
assembled = assembled.select(["features", "mpg", "carname"])

# Random test/train split.
train_df, test_df = assembled.randomSplit([0.7, 0.3])

# Define the model.
rf = RandomForestRegressor(
    numTrees=20,
    featuresCol="features",
    labelCol="mpg",
)

# Train the model.
rf_model = rf.fit(train_df)

# Make predictions.
df = rf_model.transform(test_df)

# Evaluate the model.
r2 = RegressionEvaluator(
    labelCol="mpg", predictionCol="prediction", metricName="r2"
).evaluate(df)
rmse = RegressionEvaluator(
    labelCol="mpg", predictionCol="prediction", metricName="rmse"
).evaluate(df)
print("RMSE={} r2={}".format(rmse, r2))

```
```
# Code snippet result:
+----------+----+----------+----------+
|  features| mpg|   carname|prediction|
+----------+----+----------+----------+
|[3.0,70...|18.0| maxda rx3|28.7440...|
|[3.0,70...|19.0|mazda r...|26.1376...|
|[3.0,80...|21.5|mazda rx-4|26.5120...|
|[4.0,76...|31.0|toyota ...|33.9993...|
|[4.0,78...|32.8|mazda g...|33.7947...|
|[4.0,83...|32.0|datsun 710|34.3994...|
|[4.0,85...|31.8|datsun 210|34.4604...|
|[4.0,86...|39.0|plymout...|34.9254...|
|[4.0,86...|34.1|maxda g...|34.9564...|
|[4.0,89...|29.8|vokswag...|34.8279...|
+----------+----+----------+----------+
only showing top 10 rows
```

A basic Random Forest Classification model
------------------------------------------

```python
from pyspark.ml.classification import RandomForestClassifier
from pyspark.ml.evaluation import MulticlassClassificationEvaluator
from pyspark.ml.feature import VectorAssembler

label_column = "cover_type"
vectorAssembler = VectorAssembler(
    inputCols=covtype_df.columns,
    outputCol="features",
    handleInvalid="skip",
)
assembled = vectorAssembler.transform(covtype_df)

# Random test/train split.
train_df, test_df = assembled.randomSplit([0.7, 0.3])

# Define the model.
rf = RandomForestClassifier(
    numTrees=50,
    featuresCol="features",
    labelCol=label_column,
)

# Train the model.
rf_model = rf.fit(train_df)

# Make predictions.
predictions = rf_model.transform(test_df)

# Select (prediction, true label) and compute test error
evaluator = MulticlassClassificationEvaluator(
    labelCol=label_column, predictionCol="prediction", metricName="accuracy"
)
accuracy = evaluator.evaluate(predictions)
print("Test Error = %g" % (1.0 - accuracy))
df = predictions.select([label_column, "prediction"])
```
```
# Code snippet result:
+----------+----------+
|cover_type|prediction|
+----------+----------+
|         3|       3.0|
|         3|       3.0|
|         3|       3.0|
|         3|       3.0|
|         3|       3.0|
|         6|       3.0|
|         6|       3.0|
|         6|       3.0|
|         6|       3.0|
|         6|       3.0|
+----------+----------+
only showing top 10 rows
```

Encode string variables before using a VectorAssembler
------------------------------------------------------

```python
from pyspark.ml import Pipeline
from pyspark.ml.feature import StringIndexer, VectorAssembler
from pyspark.ml.regression import RandomForestRegressor
from pyspark.ml.evaluation import RegressionEvaluator
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

# Add manufacturer name we will use as a string column.
first_word_udf = udf(lambda x: x.split()[0], StringType())
df = auto_df_fixed.withColumn(
    "manufacturer", first_word_udf(auto_df_fixed.carname)
)

# Strings must be indexed or we will get:
# pyspark.sql.utils.IllegalArgumentException: Data type string of column manufacturer is not supported.
#
# We also encode outside of the main pipeline or else we risk getting:
#  Caused by: org.apache.spark.SparkException: Unseen label: XXX. To handle unseen labels, set Param handleInvalid to keep.
#
# This is because training data is selected randomly and may not have all possible categories.
manufacturer_encoded = StringIndexer(
    inputCol="manufacturer", outputCol="manufacturer_encoded"
)
encoded_df = manufacturer_encoded.fit(df).transform(df)

# Set up our main ML pipeline.
columns_to_assemble = [
    "manufacturer_encoded",
    "cylinders",
    "displacement",
    "horsepower",
    "weight",
    "acceleration",
]
vector_assembler = VectorAssembler(
    inputCols=columns_to_assemble,
    outputCol="features",
    handleInvalid="skip",
)

# Random test/train split.
train_df, test_df = encoded_df.randomSplit([0.7, 0.3])

# Define the model.
rf = RandomForestRegressor(
    numTrees=20,
    featuresCol="features",
    labelCol="mpg",
)

# Run the pipeline.
pipeline = Pipeline(stages=[vector_assembler, rf])
model = pipeline.fit(train_df)

# Make predictions.
df = model.transform(test_df).select("carname", "mpg", "prediction")

# Select (prediction, true label) and compute test error
rmse = RegressionEvaluator(
    labelCol="mpg", predictionCol="prediction", metricName="rmse"
).evaluate(df)
print("RMSE={}".format(rmse))

```
```
# Code snippet result:
+----------+----+----------+
|   carname| mpg|prediction|
+----------+----+----------+
| ford f250|10.0|13.2290...|
|oldsmob...|11.0|16.9678...|
|ford co...|12.0|13.4351...|
|ford gr...|13.0|16.2898...|
|chevrol...|13.0|15.4739...|
|chevrol...|13.0|15.3495...|
|chevrol...|13.0|14.3390...|
|  ford ltd|13.0|13.9583...|
|chevrol...|13.0|13.7570...|
|ford gr...|14.0|15.4633...|
+----------+----+----------+
only showing top 10 rows
```

Get feature importances of a trained model
------------------------------------------

```python
from pyspark.ml import Pipeline
from pyspark.ml.feature import StringIndexer, VectorAssembler
from pyspark.ml.regression import RandomForestRegressor
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

# Add manufacturer name we will use as a string column.
first_word_udf = udf(lambda x: x.split()[0], StringType())
df = auto_df_fixed.withColumn(
    "manufacturer", first_word_udf(auto_df_fixed.carname)
)
manufacturer_encoded = StringIndexer(
    inputCol="manufacturer", outputCol="manufacturer_encoded"
)
encoded_df = manufacturer_encoded.fit(df).transform(df)

# Set up our main ML pipeline.
columns_to_assemble = [
    "manufacturer_encoded",
    "cylinders",
    "displacement",
    "horsepower",
    "weight",
    "acceleration",
]
vector_assembler = VectorAssembler(
    inputCols=columns_to_assemble,
    outputCol="features",
    handleInvalid="skip",
)

# Random test/train split.
train_df, test_df = encoded_df.randomSplit([0.7, 0.3])

# Define the model.
rf = RandomForestRegressor(
    numTrees=20,
    featuresCol="features",
    labelCol="mpg",
)

# Run the pipeline.
pipeline = Pipeline(stages=[vector_assembler, rf])
model = pipeline.fit(train_df)

# Make predictions.
predictions = model.transform(test_df).select("carname", "mpg", "prediction")

# Get feature importances.
real_model = model.stages[1]
for feature, importance in zip(
    columns_to_assemble, real_model.featureImportances
):
    print("{} contributes {:0.3f}%".format(feature, importance * 100))

```
```
# Code snippet result:
manufacturer_encoded contributes 9.159%
cylinders contributes 20.001%
displacement contributes 33.358%
horsepower contributes 11.923%
weight contributes 22.362%
acceleration contributes 3.197%
```

Automatically encode categorical variables
------------------------------------------

```python
from pyspark.ml.feature import VectorAssembler, VectorIndexer
from pyspark.ml.regression import RandomForestRegressor
from pyspark.sql.functions import countDistinct

# Remove non-numeric columns.
df = auto_df_fixed.drop("carname")

# Profile this DataFrame to get a good value for maxCategories.
grouped = df.agg(*(countDistinct(c) for c in df.columns))
grouped.show()

# Assemble all columns except mpg into a vector.
feature_columns = list(df.columns)
feature_columns.remove("mpg")
vector_assembler = VectorAssembler(
    inputCols=feature_columns,
    outputCol="features",
    handleInvalid="skip",
)
assembled = vector_assembler.transform(df)

# From profiling the dataset, 15 is a good value for max categories.
indexer = VectorIndexer(
    inputCol="features", outputCol="indexed", maxCategories=15
)
indexed = indexer.fit(assembled).transform(assembled)

# Build and train the model.
train_df, test_df = indexed.randomSplit([0.7, 0.3])
rf = RandomForestRegressor(
    numTrees=50,
    featuresCol="features",
    labelCol="mpg",
)
rf_model = rf.fit(train_df)

# Get feature importances.
for feature, importance in zip(feature_columns, rf_model.featureImportances):
    print("{} contributes {:0.3f}%".format(feature, importance * 100))

# Make predictions.
df = rf_model.transform(test_df).select("mpg", "prediction")
```
```
# Code snippet result:
+----+----------+
| mpg|prediction|
+----+----------+
|11.0|12.8097...|
|11.0|13.2055...|
|12.0|13.3509...|
|12.0|13.1467...|
|12.0|12.8700...|
|13.0|14.9091...|
|13.0|15.7799...|
|13.0|14.6798...|
|13.0|14.7337...|
|13.0|13.4198...|
+----+----------+
only showing top 10 rows
```

Hyperparameter tuning
---------------------

```python
from pyspark.ml import Pipeline
from pyspark.ml.evaluation import RegressionEvaluator
from pyspark.ml.feature import StringIndexer, VectorAssembler
from pyspark.ml.regression import RandomForestRegressor
from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

# Add manufacturer name we will use as a string column.
first_word_udf = udf(lambda x: x.split()[0], StringType())
df = auto_df_fixed.withColumn(
    "manufacturer", first_word_udf(auto_df_fixed.carname)
)
manufacturer_encoded = StringIndexer(
    inputCol="manufacturer", outputCol="manufacturer_encoded"
)
encoded_df = manufacturer_encoded.fit(df).transform(df)

# Set up our main ML pipeline.
columns_to_assemble = [
    "manufacturer_encoded",
    "cylinders",
    "displacement",
    "horsepower",
    "weight",
    "acceleration",
]
vector_assembler = VectorAssembler(
    inputCols=columns_to_assemble,
    outputCol="features",
    handleInvalid="skip",
)

# Define the model.
rf = RandomForestRegressor(
    numTrees=20,
    featuresCol="features",
    labelCol="mpg",
)

# Run the pipeline.
pipeline = Pipeline(stages=[vector_assembler, rf])

# Hyperparameter search.
target_metric = "rmse"
paramGrid = (
    ParamGridBuilder().addGrid(rf.numTrees, list(range(20, 100, 10))).build()
)
cross_validator = CrossValidator(
    estimator=pipeline,
    estimatorParamMaps=paramGrid,
    evaluator=RegressionEvaluator(
        labelCol="mpg", predictionCol="prediction", metricName=target_metric
    ),
    numFolds=2,
    parallelism=4,
)

# Run cross-validation to get the best parameters.
fit_cross_validator = cross_validator.fit(encoded_df)
best_pipeline_model = fit_cross_validator.bestModel
best_regressor = best_pipeline_model.stages[1]
print("Best model has {} trees.".format(best_regressor.getNumTrees))

# Save the Cross Validator, to capture everything including stats.
fit_cross_validator.write().overwrite().save("fit_cross_validator.model")

# Or, just save the best model.
best_pipeline_model.write().overwrite().save("best_pipeline_model.model")

```
```
# Code snippet result:
Best model has 30 trees.
```

Plot Hyperparameter tuning metrics
----------------------------------

```python
from pyspark.ml import Pipeline
from pyspark.ml.evaluation import RegressionEvaluator
from pyspark.ml.feature import StringIndexer, VectorAssembler
from pyspark.ml.regression import RandomForestRegressor
from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

# Add manufacturer name we will use as a string column.
first_word_udf = udf(lambda x: x.split()[0], StringType())
df = auto_df_fixed.withColumn(
    "manufacturer", first_word_udf(auto_df_fixed.carname)
)
manufacturer_encoded = StringIndexer(
    inputCol="manufacturer", outputCol="manufacturer_encoded"
)
encoded_df = manufacturer_encoded.fit(df).transform(df)

# Set up our main ML pipeline.
columns_to_assemble = [
    "manufacturer_encoded",
    "cylinders",
    "displacement",
    "horsepower",
    "weight",
    "acceleration",
]
vector_assembler = VectorAssembler(
    inputCols=columns_to_assemble,
    outputCol="features",
    handleInvalid="skip",
)

# Define the model.
rf = RandomForestRegressor(
    numTrees=20,
    featuresCol="features",
    labelCol="mpg",
)

# Run the pipeline.
pipeline = Pipeline(stages=[vector_assembler, rf])

# Hyperparameter search.
target_metric = "rmse"
paramGrid = (
    ParamGridBuilder().addGrid(rf.numTrees, list(range(20, 100, 10))).build()
)
crossval = CrossValidator(
    estimator=pipeline,
    estimatorParamMaps=paramGrid,
    evaluator=RegressionEvaluator(
        labelCol="mpg", predictionCol="prediction", metricName=target_metric
    ),
    numFolds=2,
    parallelism=4,
)

# Run cross-validation, get metrics for each parameter.
model = crossval.fit(encoded_df)

# Plot results using matplotlib.
import pandas
import matplotlib

parameter_grid = [
    {k.name: v for k, v in p.items()} for p in model.getEstimatorParamMaps()
]
pdf = pandas.DataFrame(
    model.avgMetrics,
    index=[x["numTrees"] for x in parameter_grid],
    columns=[target_metric],
)
ax = pdf.plot(style="*-")
ax.figure.suptitle("Hyperparameter Search: RMSE by Number of Trees")
ax.figure.savefig("hyperparameters.png")

```
```
# Code snippet result:
```
![Hyperparameter Search](hyperparameters.png)

A Random Forest Classification model with Hyperparameter Tuning
---------------------------------------------------------------

```python
from pyspark.ml.classification import RandomForestClassifier
from pyspark.ml.evaluation import MulticlassClassificationEvaluator
from pyspark.ml.feature import VectorAssembler
from pyspark.ml import Pipeline
from pyspark.ml.tuning import CrossValidator, ParamGridBuilder

label_column = "cover_type"
vector_assembler = VectorAssembler(
    inputCols=covtype_df.columns,
    outputCol="features",
    handleInvalid="skip",
)

# Define the model.
rf = RandomForestClassifier(
    numTrees=50,
    featuresCol="features",
    labelCol=label_column,
)

# Run the pipeline.
pipeline = Pipeline(stages=[vector_assembler, rf])

# Hyperparameter search.
paramGrid = (
    ParamGridBuilder().addGrid(rf.numTrees, list(range(50, 80, 10))).build()
)
crossval = CrossValidator(
    estimator=pipeline,
    estimatorParamMaps=paramGrid,
    evaluator=MulticlassClassificationEvaluator(
        labelCol=label_column, predictionCol="prediction"
    ),
    numFolds=2,
    parallelism=4,
)

# Run cross-validation and choose the best set of parameters.
model = crossval.fit(covtype_df)

# Identify the best hyperparameters.
real_model = model.bestModel.stages[1]
print("Best model has {} trees.".format(real_model.getNumTrees))
```


Compute correlation matrix
--------------------------

```python
from pyspark.ml.feature import VectorAssembler
from pyspark.ml.stat import Correlation

# Remove non-numeric columns.
df = auto_df_fixed.drop("carname")

# Assemble all columns except mpg into a vector.
feature_columns = list(df.columns)
feature_columns.remove("mpg")
vector_col = "features"
vector_assembler = VectorAssembler(
    inputCols=feature_columns,
    outputCol=vector_col,
    handleInvalid="skip",
)
df_vector = vector_assembler.transform(df).select(vector_col)

# Compute the correlation matrix.
matrix = Correlation.corr(df_vector, vector_col)
corr_array = matrix.collect()[0]["pearson({})".format(vector_col)].toArray()

# This part is just for pretty-printing.
pdf = pandas.DataFrame(
    corr_array, index=feature_columns, columns=feature_columns
)
```
```
# Code snippet result:
              cylinders  displacement  horsepower    weight  acceleration  modelyear    origin
cylinders      1.000000      0.950823    0.842983  0.897527     -0.504683  -0.345647 -0.568932
displacement   0.950823      1.000000    0.897257  0.932994     -0.543800  -0.369855 -0.614535
horsepower     0.842983      0.897257    1.000000  0.864538     -0.689196  -0.416361 -0.455171
weight         0.897527      0.932994    0.864538  1.000000     -0.416839  -0.309120 -0.585005
acceleration  -0.504683     -0.543800   -0.689196 -0.416839      1.000000   0.290316  0.212746
modelyear     -0.345647     -0.369855   -0.416361 -0.309120      0.290316   1.000000  0.181528
origin        -0.568932     -0.614535   -0.455171 -0.585005      0.212746   0.181528  1.000000
```

Performance
===========
A few performance tips and tricks.

Get the Spark version
---------------------

```python
print(spark.sparkContext.version)
```
```
# Code snippet result:
3.2.1
```

Log messages using Spark's Log4J
--------------------------------

```python
logger = spark.sparkContext._jvm.org.apache.log4j.Logger.getRootLogger()
logger.warn("WARNING LEVEL LOG MESSAGE")
```


Cache a DataFrame
-----------------

```python
from pyspark import StorageLevel
from pyspark.sql.functions import lit

# Make some copies of the DataFrame.
df1 = auto_df.where(lit(1) > lit(0))
df2 = auto_df.where(lit(2) > lit(0))
df3 = auto_df.where(lit(3) > lit(0))

print("Show the default storage level (NONE).")
print(auto_df.storageLevel)

print("\nChange storage level to Memory/Disk via the cache shortcut.")
df1.cache()
print(df1.storageLevel)

print(
    "\nChange storage level to the equivalent of cache using an explicit StorageLevel."
)
df2.persist(storageLevel=StorageLevel(True, True, False, True, 1))
print(df2.storageLevel)

print("\nSet storage level to NONE using an explicit StorageLevel.")
df3.persist(storageLevel=StorageLevel(False, False, False, False, 1))
print(df3.storageLevel)
```
```
# Code snippet result:
Show the default storage level (NONE).
Serialized 1x Replicated

Change storage level to Memory/Disk via the cache shortcut.
Disk Memory Deserialized 1x Replicated

Change storage level to the equivalent of cache using an explicit StorageLevel.
Disk Memory Deserialized 1x Replicated

Set storage level to NONE using an explicit StorageLevel.
Serialized 1x Replicated
```

Show the execution plan, with costs
-----------------------------------

```python
df = auto_df.groupBy("cylinders").count()
execution_plan = df.explain(mode="cost")
```


Partition by a Column Value
---------------------------

```python
# rows is an iterable, e.g. itertools.chain
def number_in_partition(rows):
    try:
        first_row = next(rows)
        partition_size = sum(1 for x in rows) + 1
        partition_value = first_row.modelyear
        print(f"Partition {partition_value} has {partition_size} records")
    except StopIteration:
        print("Empty partition")

df = auto_df.repartition(20, "modelyear")
df.foreachPartition(number_in_partition)
```
```
# Code snippet result:
Partition 82 has 31 records
Partition 76 has 34 records
Partition 77 has 28 records
Partition 80 has 29 records
Partition 81 has 29 records
Partition 70 has 29 records
Partition 72 has 55 records
Partition 78 has 36 records
Empty partition
Empty partition
Empty partition
Partition 75 has 30 records
Empty partition
Partition 71 has 68 records
Partition 79 has 29 records
Empty partition
Empty partition
Empty partition
Empty partition
Empty partition
```

Range Partition a DataFrame
---------------------------

```python
from pyspark.sql.functions import col

# rows is an iterable, e.g. itertools.chain
def count_in_partition(rows):
    my_years = set()
    number_in_partition = 0
    for row in rows:
        my_years.add(row.modelyear)
        number_in_partition += 1
    seen_years = sorted(list(my_years))
    if len(seen_years) > 0:
        seen_values = ",".join(seen_years)
        print(
            f"This partition has {number_in_partition} records with years {seen_values}"
        )
    else:
        print("Empty partition")

number_of_partitions = 5
df = auto_df.repartitionByRange(number_of_partitions, col("modelyear"))
df.foreachPartition(count_in_partition)
```
```
# Code snippet result:

This partition has 60 records with years 81,82
This partition has 62 records with years 76,77
This partition has 85 records with years 70,71,72
This partition has 97 records with years 73,74,75
This partition has 94 records with years 78,79,80
```

Change Number of DataFrame Partitions
-------------------------------------

```python
from pyspark.sql.functions import col

df = auto_df.repartition(col("modelyear"))
number_of_partitions = 5
df = auto_df.repartitionByRange(number_of_partitions, col("mpg"))
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
|15.0|        8|       383.0|     170.0| 3563.|        10.0|       70|     1|dodge c...|
|14.0|        8|       340.0|     160.0| 3609.|         8.0|       70|     1|plymout...|
|15.0|        8|       400.0|     150.0| 3761.|         9.5|       70|     1|chevrol...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Coalesce DataFrame partitions
-----------------------------

```python
import math

target_partitions = math.ceil(auto_df.rdd.getNumPartitions() / 2)
df = auto_df.coalesce(target_partitions)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|18.0|        8|       307.0|     130.0| 3504.|        12.0|       70|     1|chevrol...|
|15.0|        8|       350.0|     165.0| 3693.|        11.5|       70|     1|buick s...|
|18.0|        8|       318.0|     150.0| 3436.|        11.0|       70|     1|plymout...|
|16.0|        8|       304.0|     150.0| 3433.|        12.0|       70|     1|amc reb...|
|17.0|        8|       302.0|     140.0| 3449.|        10.5|       70|     1|ford to...|
|15.0|        8|       429.0|     198.0| 4341.|        10.0|       70|     1|ford ga...|
|14.0|        8|       454.0|     220.0| 4354.|         9.0|       70|     1|chevrol...|
|14.0|        8|       440.0|     215.0| 4312.|         8.5|       70|     1|plymout...|
|14.0|        8|       455.0|     225.0| 4425.|        10.0|       70|     1|pontiac...|
|15.0|        8|       390.0|     190.0| 3850.|         8.5|       70|     1|amc amb...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Set the number of shuffle partitions
------------------------------------

```python
# Default shuffle partitions is usually 200.
grouped1 = auto_df.groupBy("cylinders").count()
print("{} partition(s)".format(grouped1.rdd.getNumPartitions()))

# Set the shuffle partitions to 20.
# This can reduce the number of files generated when saving DataFrames.
spark.conf.set("spark.sql.shuffle.partitions", 20)

grouped2 = auto_df.groupBy("cylinders").count()
print("{} partition(s)".format(grouped2.rdd.getNumPartitions()))
```
```
# Code snippet result:
200 partition(s)
20 partition(s)
```

Sample a subset of a DataFrame
------------------------------

```python
df = (
    spark.read.format("csv")
    .option("header", True)
    .load("data/auto-mpg.csv")
    .sample(0.1)
)
```
```
# Code snippet result:
+----+---------+------------+----------+------+------------+---------+------+----------+
| mpg|cylinders|displacement|horsepower|weight|acceleration|modelyear|origin|   carname|
+----+---------+------------+----------+------+------------+---------+------+----------+
|27.0|        4|       97.00|     88.00| 2130.|        14.5|       71|     3|datsun ...|
|13.0|        8|       400.0|     170.0| 4746.|        12.0|       71|     1|ford co...|
|35.0|        4|       72.00|     69.00| 1613.|        18.0|       71|     3|datsun ...|
|23.0|        4|       97.00|     54.00| 2254.|        23.5|       72|     2|volkswa...|
|14.0|        8|       318.0|     150.0| 4077.|        14.0|       72|     1|plymout...|
|27.0|        4|       97.00|     88.00| 2100.|        16.5|       72|     3|toyota ...|
|29.0|        4|       98.00|     83.00| 2219.|        16.5|       74|     2|  audi fox|
|13.0|        8|       302.0|     129.0| 3169.|        12.0|       75|     1|ford mu...|
|29.0|        4|       97.00|     75.00| 2171.|        16.0|       75|     3|toyota ...|
|29.0|        4|       90.00|     70.00| 1937.|        14.0|       75|     2|volkswa...|
+----+---------+------------+----------+------+------------+---------+------+----------+
only showing top 10 rows
```

Print Spark configuration properties
------------------------------------

```python
print(spark.sparkContext.getConf().getAll())
```
```
# Code snippet result:
[('spark.jars.packages', 'io.delta:delta-core_2.12:1.1.0'), ('spark.driver.memory', '2G'), ('spark.app.startTime', '1647186922054'), ('spark.sql.warehouse.dir', 'file:/Users/cshankli/git/pyspark-cheatsheet/spark_warehouse'), ('spark.jars', 'file:///Users/cshankli/.ivy2/jars/io.delta_delta-core_2.12-1.1.0.jar,file:///Users/cshankli/.ivy2/jars/org.antlr_antlr4-runtime-4.8.jar,file:///Users/cshankli/.ivy2/jars/org.codehaus.jackson_jackson-core-asl-1.9.13.jar'), ('spark.executor.memory', '2G'), ('spark.app.initial.jar.urls', 'spark://192.168.1.199:55860/jars/org.antlr_antlr4-runtime-4.8.jar,spark://192.168.1.199:55860/jars/org.codehaus.jackson_jackson-core-asl-1.9.13.jar,spark://192.168.1.199:55860/jars/io.delta_delta-core_2.12-1.1.0.jar'), ('spark.executor.id', 'driver'), ('spark.files', 'file:///Users/cshankli/.ivy2/jars/io.delta_delta-core_2.12-1.1.0.jar,file:///Users/cshankli/.ivy2/jars/org.antlr_antlr4-runtime-4.8.jar,file:///Users/cshankli/.ivy2/jars/org.codehaus.jackson_jackson-core-asl-1.9.13.jar'), ('spark.driver.port', '55860'), ('spark.sql.extensions', 'io.delta.sql.DeltaSparkSessionExtension'), ('spark.rdd.compress', 'True'), ('spark.app.initial.file.urls', 'file:///Users/cshankli/.ivy2/jars/org.antlr_antlr4-runtime-4.8.jar,file:///Users/cshankli/.ivy2/jars/io.delta_delta-core_2.12-1.1.0.jar,file:///Users/cshankli/.ivy2/jars/org.codehaus.jackson_jackson-core-asl-1.9.13.jar'), ('spark.serializer.objectStreamReset', '100'), ('spark.driver.host', '192.168.1.199'), ('spark.master', 'local[*]'), ('spark.repl.local.jars', 'file:///Users/cshankli/.ivy2/jars/io.delta_delta-core_2.12-1.1.0.jar,file:///Users/cshankli/.ivy2/jars/org.antlr_antlr4-runtime-4.8.jar,file:///Users/cshankli/.ivy2/jars/org.codehaus.jackson_jackson-core-asl-1.9.13.jar'), ('spark.submit.pyFiles', '/Users/cshankli/.ivy2/jars/io.delta_delta-core_2.12-1.1.0.jar,/Users/cshankli/.ivy2/jars/org.antlr_antlr4-runtime-4.8.jar,/Users/cshankli/.ivy2/jars/org.codehaus.jackson_jackson-core-asl-1.9.13.jar'), ('spark.submit.deployMode', 'client'), ('spark.app.name', 'cheatsheet'), ('spark.ui.showConsoleProgress', 'true'), ('spark.sql.catalog.spark_catalog', 'org.apache.spark.sql.delta.catalog.DeltaCatalog'), ('spark.app.id', 'local-1647186922789')]
```

Set Spark configuration properties
----------------------------------

```python
key = "spark.hadoop.mapreduce.fileoutputcommitter.algorithm.version"
value = 2

# Wrong! Settings cannot be changed this way.
# spark.sparkContext.getConf().set(key, value)

# Correct.
spark.conf.set(key, value)

# Alternatively: Set at build time.
# Some settings can only be made at build time.
spark_builder = SparkSession.builder.appName("My App")
spark_builder.config(key, value)
spark = spark_builder.getOrCreate()
```


Run multiple concurrent jobs in different pools
-----------------------------------------------

```python
import concurrent.futures
import datetime
import time

other_df = auto_df.toDF(*("_" + c for c in auto_df.columns))
target_frames = [
    auto_df.crossJoin(other_df.limit((11 - i) * 20))
    .groupBy(column_name)
    .count()
    for i, column_name in enumerate(auto_df.columns)
]

def launch(i, target):
    spark.sparkContext.setLocalProperty("spark.scheduler.pool", f"pool{i}")
    print("Job", i, "returns", target.first(), "at", datetime.datetime.now())

with concurrent.futures.ThreadPoolExecutor() as executor:
    for i, target in enumerate(target_frames):
        print("Starting job", i, "at", datetime.datetime.now())
        executor.submit(launch, i, target)
        time.sleep(0.2)
```
```
# Code snippet result:

Starting job 0 at 2022-03-13 08:53:07.012511
Starting job 1 at 2022-03-13 08:53:07.213019
Starting job 2 at 2022-03-13 08:53:07.416360
Starting job 3 at 2022-03-13 08:53:07.618749
Starting job 4 at 2022-03-13 08:53:07.822736
Starting job 5 at 2022-03-13 08:53:08.023281
Starting job 6 at 2022-03-13 08:53:08.224146
Starting job 7 at 2022-03-13 08:53:08.428519
Job 1 returns Row(cylinders='3', count=800) at 2022-03-13 08:53:08.576396
Starting job 8 at 2022-03-13 08:53:08.631801
Job 6 returns Row(modelyear='73', count=4000) at 2022-03-13 08:53:08.917601
Job 7 returns Row(origin='3', count=6320) at 2022-03-13 08:53:08.941718
Job 3 returns Row(horsepower='102.0', count=160) at 2022-03-13 08:53:09.129134
Job 2 returns Row(displacement='151.0', count=1800) at 2022-03-13 08:53:09.164282
Job 0 returns Row(mpg='20.5', count=660) at 2022-03-13 08:53:09.286087
Job 5 returns Row(acceleration='8.5', count=240) at 2022-03-13 08:53:09.298583
Job 4 returns Row(weight='3574.', count=140) at 2022-03-13 08:53:09.476974
Job 8 returns Row(carname='audi 100 ls', count=60) at 2022-03-13 08:53:09.639598
```

Increase Spark driver/executor heap space
-----------------------------------------

```python
# Memory configuration depends entirely on your runtime.
# In OCI Data Flow you control memory by selecting a larger or smaller VM.
# No other configuration is needed.
#
# For other environments see the Spark "Cluster Mode Overview" to get started.
# https://spark.apache.org/docs/latest/cluster-overview.html
# And good luck!
```

### Spark

#### SQL
- One way to use Spark SQL is executing SQL Queries
- We can also interact with Spark SQL by using DataFrame API, available in Scala, Java, PYthon, and R;
- Spark SQL Engine will generate the same query plan to execute on your Spark Cluster;
#### Dataframe
A df is a distributed collection of data grouped into named columns
- A __schema__ defines the columns names and types of a DataFrame;
- __DataFrame transformations__: are methods that return a new DataFrame and are lazily evaluated. Select, where, and orderBY are examples of transformations;
  - Because each of these return a DataFrame, we are able to chain these methods together to build new DataFrames.
- __Actions:__ are methods that trigger computation. Below some examples:
  - Count returns the number of records in a DataFrame, collect returns an array o all rows in a DataFrame, and show displays the top few rows of a DataFrame;
  - These operations require Spark to execute computations to return the expected result.
- __Lazy Evaluation__ dataFrame transformations are not evaluated until an action is called
#### Spark Session
- The first step of any Spark App. It enables you to run Spark Code;
- Class provides the single entry point to all functionality in Spark using DataFrame API;
- This is automatically created for you in a Databricks notebook as the variable, spark.
  - Sql, table, read, range are all ways of using SparkSession to create a DataFrame.

__Spark SQL__ is a module used for structured data processing. 
- We can interact with Spark SQL by using the DataFrame API, available in Scala, Java, Python and R.
- You can use any interface, the SparkSQL engine will generate the same query plan.
- With the additional information Spark SQL provides on data structure and computations being performed, Spark is able to make optimizations before these computations.

__DataFrame__ is a distributed collection of data grouped into named columns. 
__Schema__ defines the column names and types of a DataFrame.
__Transformations__ are methods that return a new DataFrame and are lazily evaluated. Select, where, and orderBy are examples of transformations. Becasuse each of theses return a DataFrame, we are able to chain these methods together to build new DataFrames.



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

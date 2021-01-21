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
- __DataFrameReader__ is accessible through the SparkSession method read. This class includes methods to load DataFrames from different external storage systems.
- __DataFrameWriter__ is accessible through the sparkSession method write. This class includes methods to write Dataframes from different external storage systems.
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



-----------------


#### Closure:
Code and variables required to execute a computation in each distributed node over a partition of data. Whenever there is a task that will be executed in one of the worker nodes, the code that will be run is bundled up into a closure and a copy is distributed to each worker node. If the code in one modifies a local variable, thischange is not reflected in other nodes. There are also global variables, wich we will talk about. 
Code and variables being blundled up and sent to nodes for parallel execution.
#### Pipelines: 
We process the data in stages and we not need to wait for all data to go through each transformation to start the next one.
Is not always possible.
Narrow x Wide Transformations: 
- __Narrow__: não possuem relacionamento ou dependencias entre as partes que são processadas;
- __Wide__: possui dependencia/relacionamento com outras parte. Necessita mover o dado entre partições para serem processados juntos. (Shuffle, Suffle Boundary)

#### Stage:
Whe you subimmited an application a job is created. A job consists of multiple tasks that will be divided for processing in your cluster. Several of those tasks could be executed with the data from a single node without having to shuffle. Theses tasks that have no dependencies from other nodes can be grouped together and optimized int what is called a __stage__.

- RDD Lineage Graph:
  Graph of transformation operations required to execute when an action is called. 
 Also know as Rdd operator graph or rdd dependency graph
- Lineage: logical execution plan is represented as a directed acyclic graph
- Collect(): being an action and actions are really important because they are what triggers a computation.

### RDD
Contains a set of instructions that are used to materialize and transform  the data and generate a result with the operations being performed in parallel and parallel processing introduces a problem: what to do if while I am processing my data and a worker node fails? Spark can request that another worker node take action and process the data.

### Big Picture: Spark libraries
  - Spark SQL: concepts of DataFrame, we can work by issuing commands or using the DataFrame DSL, wich is very intuitive and easy to use.
  - __Spark Streaming__: used to handle analyses of streaming data.
  - __Mlib__: ML library build on top of spark
  - __GraphX__: distribuited graph processing library
  - __Spark Core__: foundation of the project, here is where the main functionalities of Spark live: namely scheduling, task dispatching, configuration, etc. Where we find the original abstration of  Spark, the __RDD__.
  
- Spark Architecture: has a master worker architecture
  - Driver program: context & session: is the master
  - Cluster manager: getting resources for executions and allocate them to a particular job, a job being just an application that has been subimitted. In our case, we use YARN as a cluster manager;
  - Worker node: just nodes that do the spark has a master worker architectures; Could have one or many executors.
  - Executors: is in charge of reading and writing from and to data sources and stores comptetition results in either memory or disk. Executors are created and registered whitihn the driver program and additional executors can be requested when needed .
  - Tasks: bite sized units of work that spark driver sent for execution

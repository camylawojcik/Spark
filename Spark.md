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
- __DataFrame__: just like an RDD is an immutable distributed set of data, but with the advantage that it organizes data in named columns just like a database. Data Frame also exposes an intuitive and easy-to-understand domain-specific language (DSL), that lets Spark developers think in a SQL-oriented way and work with their data. 
- __Dataset__: which basically corresponds to a strong type abstraction and in version 2 the dataset and the DataFrame APIS have been merged into one, making it much easier to work with spark. The DataFrame Api basically became an untyped dataset, namely dataset of type row and DataFrame is what we leverage when we use Spark SQL and in the same way as in RDDs, the Spark team puts a large effort in providing compatibility or an upgrade path when new versions are released.

__Datasets__ are newer, so they should be much better, therefore I should dump __DataFrames and RDDs__? Not quiet. First of all, __DataFrames__ are one special case of __Datasets__, so if you use a DataFrame, you're using the dataset API. Second, the __Dataset API__ is great work with, but some cases there might be something that you want to achieve and you may need an __RDD__. 

#### RDD or Dataset/DataFrame?
  - __Rdd__: 
    - Unstructured data;
    - Data manipulation with lambdas;
    - Low level transformations;
    - Functional;
    - __Optimization__
      - Bundle together multiple tasks into stages and pipelining.
    
  - __Dataset/DataFrame__: 
    - Structured or semi Structured data;
    - much easier to understand;
    - Equivalent to database table, but better;
    - Leverage optimizations
    - Relational style of programming;
    - Datasets have types
    Se você está começando, é mais facil usar DataFrames, especialmente se você conhece SQL 

#### Optimization
  - __Catalyst Optimizer__ is a framework that is part of __Spark SQL__.
    - It optimize queries 
      -  Objectives:
        - Allow easily add new optimization techniques
        - Enable external developers to extend it 
    - How it works:
      - Starts analyzing the queries;
      - Creates an unresolved logical plan ;
      - Looks at the catalog and resolves the references;
      - Creates an optimized logical plan (the cost-based optimization takes place and multiple plans are created and their costs are computed, selecting the best one, which is then converted into several physical plans and one is selected and we get an optimized physical plan);
      - Optimized code generation: the optimized code is generated, namely Java bytecode that will run in each worker node. 
  - __Project Tungsten__ another component that aims directly at :
    - Push the boundaries of performance
    - Optimize for CPU and memory efficiency: mainly because these are the areas that are not growing at the same rate as others, like storage and network latency;
    - Focuses on hardware where Spark runs;
    - __The main optimization features are__ to optimizes data representation in memory, providing an optimized way of storing objects in memory, specifically for Spark's requirements. 
    - Which allows for __managing memory explicitly__ in a more efficient way.
    - Cache aware computation
    - Whole-stage Code Generation: Spark can compile to bytecode several expression together instead of having to evaluate one by one each expression. This one of the improvements that allow you to gain an order of magnitude increase in performance for Spark.
    
#### SparkContext and SparkSession
__SparkContext__ we can say that is the Spark Application (RDD)
  - Located in the Spark Driver
  - Entry point for RDDs
  - The Spark Application: 
    - One SparkContext per application
    - There is an option for multiple contexts, but it's used mostly for testing and use of it is discouraged. The context is _created for you in the REPL_, but _you need to create for spark2-subimit_ for subimitting applications.
__SparkSession__: is the main _entry point for work with sparkSQL_ (DataFrames)
  - Merges SQLContext and HiveContext into one object 
  - let's you access the SparkContext
  - You can have multiple SparkSession objects
  - Created for you in REPL;
  - You need to create for spark2-submit
  
#### Spark Configuration
  - Properties: Application parameters
  - Environment Vars: per machine settings]
  - Logging: in log4j.properties
  - __Configuration Precedence__
    - spark-defaults.conf
    - passing flags to spark2-submit/spaark2-shell (overwrite those in spark-defaults)
    - Application (in code) (__have the highest precedence__)
    - You can check in the spark UI or from command line
spark2-shell --name "Super spark shell" --conf "spark.eventLog.dir=/stackexchange

  - __YARN__  Yet Another Resource Negotiator
    - Cluster management technology that came to life as well as called Hadoop Next Generation or Hadoop 2
    - __Components__:
      - Resource Manager: é o master node
      - Node Manager: cada worker node tem um NodeManager
      Driver program comunica o resourceManager sobre o job, e ele vai negocia? e determinar qual workernode irá startar o container. Esse node terá o __application Master__, e a partir dele o Resource Manager irá prover informações de onde existem recursos disponiveis para o trabalho, e então o __applicationManager__ vai se comunicar com os outros __nodeManager__ que possuem recursos disponiveis e os containers serão criados. Eles são chamados de __Spark Executors__, registrados no __ApplicationMaster__ e performando a tarefa que recebem do Spark Driver. __Dinamyc Allocation__: quando novos executors são requisitados para tarefa e são devolvidos após o termino. FIFO, First Schedule.

### RDD
__SparkContext__ we can say that is the Spark Application (RDD)
  - It serves as the master
  - It provides and lets you configure all necessary parameters regarding your application's configuration and environment and sets up all the internal services required for your app to run.
  - provides the methods to help you create RDDs to start transformations to your data;
__ 5 Properties of an RDD__
  - Partitions: quais são as que formam o rdd?
  - Dependencies: qual é o parent RDD que este RDD depende?
  - Functions to compute partition: quais são as funões aplicadas para computar a partição?
  - Partitioner (key/value RDDs): which tells which partitioner is used to compute a partition for any RDD that's in the form of key value pair.
  - Preferred locations for compute: can specify where the partition should live.
__Pair RDD__: data is arranged in a key value pair, each entry is a tuple; useful for grouping or aggregating; can use RDD transformations; there are specialized transformations available; 
__Creating RDD__: 
  - Parallelize: will create an RDD in memory from data that you provide or that you specify;
  - External Data: you might store your data in HDFS, a local file system, or S3, reading your data is usually one of the ways which you start processing it and third from another RDD;
  - From another RDD: dataset or dataframe
#### Funções
  - Repartitions: can be used to specifythe new number of partitions; ncrease or decrease
  - Coalesce: only decrease
  
__SparkSession__: is the main _entry point for work with sparkSQL_ (DataFrames)

#### Actions:
They indicates to Spark that it is time to start processing 

#### Partitions:
Is a bunch of data, just a logical grouping 
  - One of the foundations of parallelism 
  - Faster to operate whithin partition
  - Group data to minimize network traffic
  Data Locality:
    - when running in YARN it will read in from HDFS blocks when possible, next to the executors;
    - Spark determine its resources and create partitions based on how many executors it has available. 
    - You can drive this via configuration or parameters;
    Spark determines how to shuffle the data using a partitioner
    - For example: it can use a hash partitioner or range partitioner to specify which element should go.
    Repartition: you can specify
  __More or less partitions?__
    - More partitions = less data per partitions
      - smaller jobs
      - More parallelism
      Ok if you have more worker nodes and ther overload to start a job is not a large part of the work that it takes for each task
    - Less partitions= more data per partitions
      - larger jobs
      - totally fine if larger chunks of data can be processed efficiently with the worker nodes that you have available
#### Going Deeper
  - ReduceByKey: a operação é executada localmente, ou seja, menos dados são embaralhados
  - groupByKey: usar com cuidado
  - aggregateByKey
  -__Histogram__ helps when performing statistical data exploration. (It's a diagram consisting of rectangles whose area is proportional to the frequency of a variable and whose width i equal to the class interval). Helps understand the meaning of the data. (Graphic tool) 
  - __Cache__ you can persist in disk, memory or both; specially usefuk for reusing results from complex calculations or where a lot of shiffling is required.
  
  - combineByKey:
      
    

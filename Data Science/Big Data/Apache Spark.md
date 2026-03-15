> Motivation: reduce the amount of Disk I/O in Hadoop with **In-Memory Data Sharing**

# Foundation
- _Spark: Cluster Computing with Working Sets_  
    Matei Zaharia, Mosharaf Chowdhury, Michael J. Franklin, Scott Shenker, Ion Stoica  
    USENIX HotCloud (2010)  
    people.csail.mit.edu/matei/papers/2010/hotcloud_spark.pdf
- _Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing_  
    Matei Zaharia, Mosharaf Chowdhury, Tathagata Das, Ankur Dave, Justin Ma, Murphy McCauley, Michael J. Franklin, Scott Shenker, Ion Stoica  
    NSDI (2012)  
    usenix.org/system/files/conference/nsdi12/nsdi12-final138.pdf
# Spark
## Structure
4 Packages:
	Spark SQL
	Streaming
	SparkML: traditional machine learning algos
	Graphx: graph algos
Cluster manager: MESOS, YARN

Spark Program:
	driver program
	worker program
## DataFrame
A **DataFrame** is partitioned to be stored in distributed nodes, and each **partition** stores some rows of the table. DataFrame objects are designed to mimic python pandas.
```python
flightData2015 = spark\
	.read\
	.option("inferSchema", "true")\
	.option("header", "true")\
	.csv("shared/spark-guide/data/flight-data/csv/2015-summary.csv")
```
## pySpark
### Setup
```python
import os
import pyspark
from pyspark.sql import SparkSession
from pyspark.sql.functions import col

conf = pyspark.SparkConf()
# conf.set('spark.ui.proxyBase', '/user/' + os.environ['JUPYTERHUB_USER'] + '/proxy/4041')
# conf.set('spark.sql.repl.eagerEval.enabled', True)&nbsp;
conf.set('spark.driver.memory','8g')

spark = SparkSession.builder.config(conf=conf).getOrCreate()
sc = spark.sparkContext
```
### Shared Variables
`sc.broadcast`
	Efficiently send large, read-only value to all workers
	Saved at workers for use in one or more Spark operations
`sc.accumulators`
	Aggregate values from workers back to driver
	Only driver can access value of accumulator
	For tasks, accumulators are write-only
> Old, Java based
# Foundation
3 Google papers:
	2003 The Google File System
	2004 MapReduce: Simplified Data Processing on Large Clusters
	2006 Bigtable: A Distributed Storage System for Structured Data

Layers of Abstraction:
	Applications
	MapReduce (Algorithm)
	Bigtable (Data Model)
	GFS (File System)
# Hadoop Cluster
Interact With Hadoop Cluster:
Client Machine<---> Login Machine <---> Hadoop Cluster
## Hadoop Common
Infrastructures for other Hadoop components 
## Resource Manager: YARN

|                 | Master Node                  | Slave Node      |
| --------------- | ---------------------------- | --------------- |
| MapReduce Layer | Job Tracker, TT Task Tracker | TT Task Tracker |
| HDFS layer      | Name Node, DN Data Node      | DN Data Node    |
## Distributed Data Storage: HDFS Hadoop Distributed File System
**Name Node**: the *master* machine that tracks the data
	Name Node is rack-aware: which rack has which data node
	Name Node knows the data are on which data nodes
**Data Node**: the *slave* machine

Web UI: `http://<hadoop IP>:50070`
```bash
hdfs version
hdfs dfs
	-put # files in login machine => hadoop cluster
	-get # files in hadoop cluster => login machine
	-ls
	-mkdir
	-rmr
```
## Distributed Data Processing: MapReduce
**Job Tracker**: the *master* process that tracks the jobs
**Task Tracker**: the *slave* process
### Data types
**Writable**: defines a de/serialization protocol, all data types in Hadoop is a Writable
**WritableComparable**: defins a sort order, all keys is a WritableComparable
**IntWritable, Text, LongWritable, etc**: Concrete classes for different data type
**SequenceFiles**: Binary encoded of a sequence of key/value pairs
### MapReduce
Note:
k1: input keys, k2: intermediate keys, k3: output keys
Keys are comparable, not only 可判等的

In Map Machines:
	**map**: (k1, v1) -> (k2, v2)\*, take in a key-value pair, and returns any number of key-value pairs
	**combine**: Mini-reducers that run in memory locally after the map phase in order to reduce network traffic
	**partition**: (k2, number of partitions) -> partition for k2, Often a simple hash of the key, e.g., hash(k’) mod n; it basically assigns keys for each reducer
		partition is deterministic, and hence can be performed on different machines 

In Reduce Machines: Data of the same key cannot go into different reducers.
	**shuffle**: Reduce machines download key value pairs assigned to them.
		output is stored as files locally on reduce machine
	**sort**: multiple (k2, v2/Sequence(v2))\* -> (k2, Sequence(v2))\*, sort key value pairs by keys (essentially aggregating values by keys)
		output is stored as files locally on reduce machine
	**reduce**: (k2, Sequence(v2)) -> (k3, v3), take in a key-value sequence pair, and returns any number of key-value pairs
		output is stored on HDFS
#### Caveats & Constraints
Avoid object creation (reuse the object instead): costly, can trigger garbage collection a lot
Avoid buffering (use a stream): not scalable
Not good for algorithms that need Multiple Passes Over Data, e.g. PageRank
Does not maintain a global state, e.g. K-means
#### Implementation
```bash
mapred streaming \
	-input input_data_file \
	-output output_data_file \
	-mapper "python mapper.py" \
	-reducer "python reducer.py" \
	-files mapper.py,reducer.py # temp code

hdfs dfs -put ./* midterm
hdfs dfs -get midterm/*

yarn logs \
  -applicationId application_1740271921940_16292 \
  -log_files stderr \
  | grep -E "\[ERROR\]|\[WARN\]|\[FATAL\]" -A5

hdfs dfs -rm -r midterm/*
rm ./mapper1_3.py ./reducer1_3.py

# 1.1
mapred streaming \
  -input midterm/temperature-data.txt \
  -output midterm/output1_1 \
  -mapper "python mapper1_1.py" \
  -reducer "python reducer1_1.py" \
  -file mapper1_1.py \
  -file reducer1_1.py

# 1.2
mapred streaming \
  -input midterm/output1_1 \
  -output midterm/output1_2 \
  -mapper "python mapper1_2.py" \
  -reducer "python reducer1_2.py" \
  -file mapper1_2.py \
  -file reducer1_2.py

mapred streaming \
  -D mapreduce.job.reduces=1 \
  -input midterm/output1_2 \
  -output midterm/output1_3 \
  -mapper "python mapper1_3.py" \
  -reducer "python reducer1_3.py" \
  -file mapper1_3.py \
  -file reducer1_3.py

/home/yz10590_nyu_edu/output1_3/part-00000
```
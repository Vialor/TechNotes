# CAP Theorem: The impossible triangle
**CAP Theorem**: a distributed data store can only offer two of these three: Consistency, Availability, Partition Toleration
	**Consistency**: Data on all nodes are the same and updated
	**Availability**: The system is responsive to the requests
	**Partition Tolerance**: The system works despite network partition (some nodes lose connection)

**BASE Basically Available, Soft state, Eventual consistency**
	**Basically Available**: The service is still available despite partial failures and network partitions, but the consistency is not guaranteed
	**Soft State**: Even without an input, the system states may be different among nodes
	**Eventual Consistency**: A trade off between consistency and availability; the system will become consistent over time when no new input
# Apache Kafka
Producers, Brokers, Consumers
每个 **topic** 可以被划分成多个 **partition**。每个 partition 是一个有序的、不可变的**message queue**消息队列。
**Consumer groups** are formed to consume the same topic. One topic is broke down to multiple partitions, and each consumer in the group will pick some of the partition to consume. One partition can be consumed by multiple consumer groups; each group has its own offset to keep track.
	Motivation: 并发消费，负载均衡

# 5 types of database
**Key value Database**: Hashing
	Redis server, Scalaris, Amazon DynamoDB.
**Document-based Database**: It is like a KV database with document knowledge in the value part
	[[MongoDB]](based on json), CouchDB
**Column-based Database**: Columns(rows for traditional SQL) are stored consecutively on disk
	Apache Cassandra, HBase
	Comparing to traditional SQL: good for OLAP, bad for OLTP
**Graph-based Database**
	Neo4j, Amazon Neptune, ArangoDB
**Vector Database**
	ChromaDB
# Features 
Runs well on clusters (for cloud)
Built for new generation web application
Schema-less

No need for separate caching layer
Continuous availability and no single point of failure
High performance with linear scalability 
# Vector Database
2 retrieval approaches for vector indexing:
	Exhaustive: Guaranteed exact retrieval by similarity
		flat index
	Non-exhaustive: No guarantee of exact retrieval by similarity
		potential algos: ANN Approximate Nearest Neighbors
			tree-based index, graph-based index, quantization-based index, hash-based index
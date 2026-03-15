Three Layers of **Index Organization**: Index Structure, Index File, underlying tables
	Also **File Organization**: relations in underlying tables are distributed in the index structure
**Search Key**: an attribute or a set of attributes used to look up records in a file
**Index File**: an index file consists of **index entries** of the form `search key: pointer` (The leaf nodes of B+ tree)
# Types of indices
> Indexing should be used for fields that have *high selectivity (many different values) and low frequency to update*

**Ordered indices**: search keys are stored in sorted order
- **Primary index / Clustered index** 聚簇索引: search key that determines the physical order of data in a database table
  **Secondary index / Non-clustered index**: search key that *does not* determines the physical order of data in a database table
- **Dense index**: index record appears for every row in the DB
  **Sparse index**: contains index records for only some rows (applicable on clustered index only)
**Hash indices**: search keys are distributed uniformly across *buckets* using a *hash function*

**Single Key**
**Composite Keys**
	(age, salary) or (salary, age) Ordered indices
	hash (age, salary)
	2D data structure (grid file, quad-trees and etc)

**Bitmap indices**:  a bitmap for each categorical value, each bit in bitmap records whether a row has that categorical value
	Used when limited amount of categorical values, and AND/OR/NOT operations are used a lot
## 回表
**回表**：二级索引的叶节点存的指针并不指向数据，而是该行数据的主键（减少数据的位置发生变化时，索引的更新）。找到主键后再以主键进行查询，如此经历了两次B+树就是回表。
**覆盖索引**：如果查询中涉及的所有字段都能够被二级索引覆盖，那么可以直接从二级索引返回结果，从而避免了回表。
# Indexing structures
## Ordered indices: B+ tree
Degree >> 2, Data in leaves only, Same depth everywhere, 
leaf nodes are connected like a linked list, which forms the **Index File**
Each node has this format: P1, K1, P2, K2 ... Kn, P(n+1), Ki are search-key values, Pi are pointers to nodes or data

**Why not just do binary search?**
binary search is not I/O-efficient; B+ tree fetches only 4-5 index nodes and can cache the top levels
**Operations**: delete insert read
## Hash indices
Use hash function to assign index entries in to buckets
### Extendible hashing
#### Motivation
Solve the scalability issue of increasing hash collisions
#### Implementation
*A hashing function to map search keys to $[0,1,2,…, 2^n-1]$* (n is a large number, for example 32)
*A global variable of a list of match patterns* similar to IP. This variable determines how many bits of the hash value are used to index into the directory.
## Bitmap Indices
How: For each value of an attribute, there is an array of bits. The arrays tracks whether each row has that value.
When: used when limited amount of values for the attribute, and when queries on multiple attributes
Why: can take advantages of bitmap operations for queries on multiple attributes

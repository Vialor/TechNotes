parser & translator: SQL query => Relational Algebra Expression
optimizer: Relational Algebra Expression => Query execution plan (represented as **query plan tree**)
evaluation engine runs the query execution plan: data => query output
# Query Optimization
Evaluation Plan
Equivalence rules
Cost Estimation
## Selection
Linear search
Cost estimate = br block transfers + 1 seek
## Join Optimized algorithms
### Challenges
Small/Large Join, Large/Large Join
Equality-join, Inequality-join
### Solution
Most common: **Index nested-loop join**, along foreign keys (for small/large joins)
Next: **Block nested-loop join** (for small/large joins)
Rare: **Sorted merge join** and **Hash join** (for equality and large/large joins)
#### Naive nested loop join
```python
for each tuple tr in r:
	for each tuple ts in s:
		if (tr,ts) satisfy the join condition:
			add tr • ts to the result.
```
#### Index nested-loop join
```python
# r is usually much smaller than s
for each tuple tr in r:
	find tuple ts in s using the index strucuture of s such that (tr,ts) satisfies the condition
	add tr • ts to the result.
```
#### Block nested-loop join
```python
# join r and s
for each block Br in r:
	for each block Bs in s:
		for each tuple tr in Br:
			for each tuple ts in Bs:
				if (tr,ts) satisfy the join condition:
					add tr • ts to the result.
```
#### Sorted merge join
1. Sort both relations on their join attribute
2. Merge the sorted relations to join them
	careful about duplicate values: every pair with same value on join attribute must
be matched
#### Hash join
Hash function *h* maps *join attributes* values to {0, 1, ..., n},
Table r is partitioned into ri. Each tuple tr is put in partition ri where h(join attributes in tr) = i. Same for table s.
For each i, join ri and si
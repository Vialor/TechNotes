# Motivation and Complexity

> maintain some feature of an array in log(n) _update_ and _range query_

| (original array has N elements) | Q range queries | Q updates |
| ------------------------------- | --------------- | --------- |
| brute force                     | O(NQ)           | O(Q)      |
| pre sums                        | O(Q)            | O(NQ)     |
| BIT/segment tree                | O(QlogN)        | O(QlogN)  |
# Implementation
> BIT is simpler than Segment tree in terms of data structure and memory usage, but BIT is less powerful because it only supports prefix range queries.
## Binary Indexed Tree BIT
root: t\[-1]
for node tree\[x]:
	covers an interval of \[x - lowbit(x) + 1, x]
	parent node: t\[x + lowbit(x)]
	previous interval node: t\[x - lowbit(x)]
	Note: lowbit(x) = i & -i = the value of the rightmost 1 in binary x
```Python
class BIT: # max version (also min and sum)
	def __init__(self, n: int):
		self.tree  = [-inf] * n
	
	# update max: log(n)
	def update(self, i: int, val: int) -> None:
		while i < len(self.tree):
			self.tree[i] = max(self.tree[i], val)
			i += i & -i # to parent
	# returns the max: log(n)
	def preMax(self, i: int) -> int:
		mx = -inf
		while i > 0:
			mx = max(mx, self.tree[i])
			i -= i & -i # to prev interval node
		return mx
```
## Segment Tree
#TODO
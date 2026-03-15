# 2 Elements of Greedy Algorithms
**Optimal substructure** (The DP part): optimal solution to the problem contains optimal solution to its sub-problem
**Greedy choice property** (The greedy part): choose the best option at each step can lead to an overall optimal solution (OR: every decision we make does not exclude the possibility of the optimal solution.)

有些元素可以优先产生相应答案而不会对后面的元素产生影响。贪婪算法的贪婪体现在”急于过早”给出答案。
# Maximum Disjoint Intervals
> Find a largest subset of a set of intervals S, such that there is no overlaps 

Leetcode 435 (452)
```python
def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
	curEnd = float("-inf")
	count = 0
	for l, r in sorted(intervals, key=lambda l : l[1]): # important
	  if l < curEnd:
		count += 1
	  else:
		curEnd = r
	return count
```

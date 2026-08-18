# Interface
Insert a new interval x: O(log(n))
Remove interval x: O(log(n))
Search for all/any intervals that overlap interval x: O(log(n)+k) where k is the number of intervals that overlap with the given interval x
# Implementation
```Python
class IntNode:
	def __init__(intlow: int, inthigh: int):
		self.intlow = intlow
		self.inthigh = inthigh
		
		self.left = None
		self.right = None
		
		self.intmax = inthigh # right most point for inthigh in the subtree
```
Interval Tree is ordered by `intlow`
```Python
def searchAll(x, root):
	if not root:
		return
	ans = []
	def DFS(node):
		if node.inthigh >= x.intlow and x.inthigh >= node.intlow:
			nonlocal ans
			ans.append(node)
		if node.right and x.inthigh >= node.right.intlow:
			DFS(node.right)
		if node.left and x.intlow <= node.left.intmax:
			DFS(node.left)
	DFS(root)
	return ans
```
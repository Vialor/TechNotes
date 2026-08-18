# DFS Traversals
> DFS Traversals can find out: height, depth of all nodes and the size of all subtrees

Pre Order: cur → left → right, search nodes with small depth first
In Order: left → cur → right, by the order of BST
Post Order: left → right → cur, search nodes with large depth first
# BFS Traversals
```Python
# LC102
def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
	curLevel = [root]
	ans = []
	while len(curLevel) > 0:
		ans.append([node.val for node in curLevel if node])
		nextLevel = []
		for node in curLevel:
			if node is not None:
				nextLevel.append(node.left)
				nextLevel.append(node.right)
		curLevel = nextLevel
	return ans[:-1]
        
# LC297 Serialization (with some modification)
def serializeTree(self, root):
	res = []
	BFSQueue = collections.deque([root])
	while BFSQueue:
		node = BFSQueue.popleft()
		if node:
			res.append(node.val)
			BFSQueue.append(node.left)
			BFSQueue.append(node.right)
		else:
			res.append(None)
	while res and res[-1] is None:
		res.pop()
	return res
        
def deserializeTree(self, data):
    if len(data) == 0:
        return None
    i = 0
    head = TreeNode(val=data[i])
    BFSQueue = collections.deque([head])
    while i < len(data):
        node = BFSQueue.popleft()
        if node:
            i += 1
            node.left = TreeNode(val=data[i]) if i < len(data) and data[i] is not None else None
            i += 1
            node.right = TreeNode(val=data[i]) if i < len(data) and data[i] is not None else None
            BFSQueue.append(node.left)
            BFSQueue.append(node.right)
    return head
```
# Predecessor/Successor
## In Order Predecessor

```python
def inorderPred(node):
	# 如果节点有左子树 前驱是左子树中最右边的节点
    if node.left:
        predecessor = node.left
        while predecessor.right:
            predecessor = predecessor.right
        return predecessor
    # 如果节点没有左子树 则寻找祖先节点
	while node.parent:
		# 如果当前节点是父节点的右子节点 父节点就是前驱
		if node.parent.right == node:
			return node.parent
		node = node.parent
	return None
```
# Moris
## Morris In Order
```Python
while cur is not None:
	if cur.left is None: # left is None
		visit(cur)
		cur = cur.right
	else:
		pred = inorderPred(cur)
		if pred.right is None: # left is not done, first time cur
			pred.right = cur
			cur = cur.left
		else: # left is done, second time cur
			pred.right = None
			visit(cur)
			cur = cur.right

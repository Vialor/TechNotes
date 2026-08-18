```Python
def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
		result = []
		stack = [root]
		while stack:
		    curNode = stack.pop()
		    if not curNode: continue
		    stack.append(curNode.right)
		    stack.append(curNode.left)
		    result.append(curNode.val)
		return result
		
def inorderTraversalRecursionBased(self, root: Optional[TreeNode]) -> List[int]:
    result = []
    stack = [root]
    while stack:
        curItem = stack.pop()
        if curItem is None: continue
        if isinstance(curItem, int):
            result.append(curItem)
            continue
        stack.append(curItem.right)
        stack.append(curItem.val)
        stack.append(curItem.left)
    return result
    
def inorderTraversalDFSPathBased(self, root: Optional[TreeNode]) -> List[int]:
		result = []
		stack = []
		rightNode = root
		while True:
			  while nextNode:
					  stack.append(root)
					  nextNode= nextNode.left
			  if not stack:
				    return result
			  curNode = stack.pop()
			  nextNode = curNode.right
				result.append(curNode.val)
				
# path based stack looks terrible
def postorderTraversalDFSPathBased(self, root: Optional[TreeNode]) -> List[int]:
        result = []
        stack = []
        while root:
            stack.append(root)
            if root.left:
                root = root.left
            else:
                root = root.right
        while stack:
            curNode = stack.pop()
            result.append(curNode.val)
            if not stack: return result
            if curNode == stack[-1].left:
                nextNode = stack[-1].right
                while nextNode:
                    stack.append(nextNode)
                    if nextNode.left:
                        nextNode = nextNode.left
                    else:
                        nextNode = nextNode.right
        return result	
```
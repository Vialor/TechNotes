# Basic Concepts
**Depth of a node:** the number of edges from root to this node
**Height of a node:** the number of edges on the longest path from the node to a leaf
**Height of a tree:** the height of root
**Size of a tree:** the number of nodes in the tree
**Diameter of a tree**:  the length of the longest path between any two nodes in a tree

For a tree to do search, insert & delete within log(n) time, it has to be ==**ordered & self-balanced**==
# Binary Tree
[[Binary Tree Traversals]]

**Binary Search Tree BST:** an ordered binary tree in which each node has node.left.val < node.val < node.right.val
**Balanced Binary Tree/Height-balanced Binary Tree:** a binary tree in which the **depth** of the two subtrees of every node differs by less or equal than one.
	the **size** of the two subtrees of every node differs by less or equal than one --> the binary tree is **height-balanced**
[[Red Black Tree]]
[[AVL Tree]]
# B Tree
**2-3 Tree**: B tree with only 2-node and 3-node. There is also **2-3-4 trees**.

B Tree is a generalized BST:
B tree is *ordered*:
	2-node: node.left.val < node.val < node.right.val
	3-node: node.left.val < node.val1 < node.mid.val < node.val < node.right.val 
	...
	n-node
B tree is *self-balanced*: all leaf nodes have the same distance from root
B tree with degree t: Every nodes except the root hold between t-1 and 2t-1 keys; the root can have 1 to 2t-1 keys.

**Preemptive split**: While inserting a key, when a node is encountered that is already at its maximum capacity (but not over capacity), it is split immediately before proceeding to its children for further traversal or insertion.
	Motivation: reduce disk access since it avoids potential backtracking to the root
# Augmented Tree: aggregation
Add an attribute for each node storing an aggregating value (min/max/sum) of *certain variable* about the subtree
	*certain variable*: depth/height, node value, node number count
	`node.aug_attr = min/max/sum(node.left.aug_attr, node.right.aug_attr)`

[[Order Statistics Tree]]
[[Interval Tree]]
augment the sum of values in the subtree --> in log(n), find the in order presum
# Other Applications
**Trie (Prefix Tree) 前缀树/字典树**: each node stores a letter, a downward path from root to leaf forms a word
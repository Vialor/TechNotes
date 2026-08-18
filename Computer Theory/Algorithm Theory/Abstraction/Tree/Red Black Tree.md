O(logn) Insertion, Search ＆Deletion
# Basic rules
1) Every node is colored red or black 
2) The root is black 
3) Every leaf is black and doesn’t contain any data 
4) Both children of a red node are black 
5) **Black property**: For every node in the tree, all paths from that node down to a leaf (nil node) have the same number of black nodes along the path
## Corollaries
**Black height** of a node: black nodes encountered on a path to a leaf (nil) node, not including the node itself.
A node with height h has black height at least h/2.
The height of a red black tree is no more than 2lg(n+1).
# Red black tree and 2-3-4 tree are equivalent
Encoding 4-nodes as three nodes: two red, one black. The black node will be the parent of the red children
Encoding 3-nodes as two nodes: one red , one black. The red node will be the left (or right) child of the black node
Encoding 2-nodes as one node: color the node black.

A 2-3-4 tree can have multiple corresponding RB trees because of the two encodings of 3-nodes
# Coding Tricks
make a singly sentinel, T.nil for all the leaves, and for T.nil to also be the root’s parent.
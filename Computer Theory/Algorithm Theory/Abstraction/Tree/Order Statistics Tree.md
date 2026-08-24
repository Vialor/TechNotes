# Interface:
Remove node: log(n)
Insert node: log(n)
Find the i-th in-order node: log(n)
Find the in-order index of node x: log(n)
# Implementation
> Augment each node with the size of subtree

```python
def select(root, i):
    r = (root.left.size if x.left else 0) + 1
    if i == r:
        return root
    elif i < r:
        return select(root.left, i)
    return select(root.right, i - r)

def rank(root, x):
    r = (x.left.size if x.left else 0) + 1
    while x != root:
        if x == x.parent.right:
            r += (x.parent.left.size if x.parent.left else 0) + 1
        x = x.parent
    return r
```

Alternative implementation: augmenting the data structure by adding at each node the rank of that node of the subtree of which it is the root
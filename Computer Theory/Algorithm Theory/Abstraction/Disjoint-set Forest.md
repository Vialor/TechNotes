# Definition
Disjoint-set Forest(or just disjoint sets) implements:

findRoot(e)
find the oldest ancestor of the given element and do *path compression* at the same time
	*path compression*: 在向上查询的同时，把在路径上的每个节点都直接连接到根上
average complexity O(α(n)), α为反阿克曼函数，是一种优于logn的复杂度
	
union(e1, e2)
merge the sets of e1 and e2, so they are now a single set
O(α(n))
	
add(e)
add an element whose root is itself, it is a trivial operations.
O(1)
# Union-Find Algorithm
```Python
class DisjointSet:
    def __init__(self):
        self.parent = [i for i in range(n)]
        
    def findRoot(self, e):
        if self.parent[e] == e:
            return e
        self.parent[e] = self.findRoot(self.parent[e])
        return self.parent[e]
        
    def union(self, e1, e2):
        r1 = self.findRoot(e1)
        r2 = self.findRoot(e2)
        self.parent[r1] = r2
```
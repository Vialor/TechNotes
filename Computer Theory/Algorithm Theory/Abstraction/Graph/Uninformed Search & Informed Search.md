# Uninformed Search
**DFS Depth First Search**: Space O(branching factor * depth) Time O(V + E)
	**discovery time**: the moment to call DFS(node)
	**finishing time**: the moment done calling DFS(node)
	In a *directed graph*, edge u -> v has only 4 categories: (b: finishing time, d: discovery time)
		**tree edge**(v is a child of u in DFS) or **forward edge**(v is a indirect descendant of u in DFS) iff u.d < v.d < v.f < u.f
		**back edge**(v is an ancestor of u in DFS) iff v.d <= u.d < u.f <= v.f
		**cross edge**(u and v have no ancestor-descendant relationship in DFS) iff v.d < v.f < u.d < u.f
**BFS Breadth First Search**: Space O(branching factor ** depth) Time O(V + E)
## General Iterative solution
```Python
frontier = [root]
# frontier data structure:
# queue: Breadth First Search
# stack: Depth First Search
# priority queue: searches that prioritize the min/max (e.g. Dijkstra's Algorithm)
def GraphSearch():
	if isGoal(root):
		return root
	while not empty(frontier):
		node = frontier.pop()
		for each child in expand(node):
			if isGoal(child):
				return child
			frontier.add(child)
	return None
```
## DFS recursive solution
```Python
path = [] # a stack of the history of nodes
def DFS(node): # i denotes the progress of current DFS
	if isGoal(child):
		return child
	for each child in expand(node):
		path.append(child)
		DFS(child)
		path.pop()
DFS(0)
```
# Informed Search
**Informed(Heuristic) Search** can determine one state is better another in reaching the goal state

> node := <current state, parent node, action, path cost>  
> h: heuristic function, Heuristic of goal states is generally set to 0.  

**Best first searches**: Greedy Best First Search and A* Search
## Greedy Best First Search

> Dijkstra’s search with f = h  
> not optimal  
## A* Search

> Dijkstra’s search with f = g + h  
> g : known cost, h*: true cost in the future  
> optimal  

h is **admissible** if for all n, h(n) ≤ h*(n) (so h(optimal state) = 0)  
h never overestimates  
h is **consistent/monotonic** if for any neighboring n and n’, h(n) - h(n’) ≤ actual cost from n to n’ (f(n) ≥ f(n’))  
f is non-decreasing along any path  
each step of h never overestimates  
> consistent ⇒ admissible  
> admissible ⇒ optimal  
## Weighted A* Search

> f(n) = g(n) + w * h(n)  
> w * h(n) may be inadmissible  
> not optimal  

**Sacrificing Search**: search for a suboptimal solution for efficiency
# Optimization
## Iterative searching
**depth limited search**: use DFS but cutoff when the depth has reached the limit
**iterative deepening search**: do depth limited search with increasing limit until we find the answer
## Decision tree pruning
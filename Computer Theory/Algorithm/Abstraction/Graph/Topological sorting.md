> Find a total order of nodes in a dependency graph, where the graph is *directed acyclic*.

A --> B, B depends on A
# BFS Solution

```python
# LC 207
def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
	# a is the prerequisite of courses in graph[a]
	graph = [[] for _ in range(numCourses)]
	# unsatisfiedNum[a] contains unsatisfied number of prerequisites for a
	unsatisfiedNum = [0 for _ in range(numCourses) ]
	for course1, course2 in prerequisites:
		graph[course2].append(course1)
		unsatisfiedNum[course1] += 1
		
	frontier = [i for i in range(numCourses) if unsatisfiedNum[i] == 0]
	count = 0
	while frontier:
		popCourse = frontier.pop()
		for course in graph[popCourse]:
			unsatisfiedNum[course] -= 1
			if unsatisfiedNum[course] == 0:
				frontier.append(course)
		count += 1
	return count == numCourses
```
# DFS Solution
```python
TOPOLOGICAL-SORT(G):
	call DFS(G)
	as each vertex is finished, insert into front of a linked list
	return the linked list
```
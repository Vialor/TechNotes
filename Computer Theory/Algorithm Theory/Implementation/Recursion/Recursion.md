# Categories of recursions
**Directed Recursion:**
Linear Recursion: a function that makes only a single call to itself each time the function runs
Head Recursion: A call is head-recursive when the first statement of the function is the recursive call
Tail Recursion: A call is tail-recursive when the last statement of the function is the recursive call
Exponential Recursion: Recursion where more than one call is made to the function from within itself.
Nested Recursion: One of the arguments to the recursive function is the recursive function itself.
**Indirect Recursion:** More than one functions calling each other in a circular manner
# Directed Recursion

> **TCO (Tail Call Optimization)**: The ability to avoid allocating a new stack frame for a function because the calling function will simply return the value that it gets from the called function  
> It is implemented in the compilers of many languages, Python is not one of them  

> Generally speaking, Linear Recursions can be rewritten as Tail Recursion with auxiliary parameters

[[Any recursion can be rewritten as stacks - tree traversals]]
```Python
def recursive_function():
	# base cases
	# Inductive step, which includes recursive_function
```
## Subset Problems
Binary Recursion, for each element: choose it or not
```Python
n = len(s)
path = []
def DFS(i):
	# i: s[:i] has been solved, we are looking at s[i]
	if i == n:
		return
	DFS(i+1)
	path.append(s[i])
	DFS(i+1)
	path.pop()
DFS(0)
```
for each element in the answer: choose which element from the rest
```Python
n = len(s)
path = []
def DFS(i):
	# i: s[:i] has been solved, we are looking at s[i]
	if i == n:
		return
	for j in range(i, n):
		path.append(s[j])
		DFS(j+1)
		path.pop()
DFS(0)
```
# Tips
1. Results of recursive functions can be **cached** with a hash map(`@cache` in Python). An idea of Dynamic Programing.
2. Define a recursive helper function that solves a more difficult problem, and then use the solution to the difficult problem to solve the original problem.
    
    Can be achieved through recursive helper function returning more information.
    
3. Pointers to the objects of the same class are recursive in nature. These includes linked lists and trees.
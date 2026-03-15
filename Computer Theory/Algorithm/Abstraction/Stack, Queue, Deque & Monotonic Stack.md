# Stack, Queue, Deque
## Definition
**Stack** FILO
`pop` `push` `peek` O(1)
**Queue** FIFO
`dequeue` `enqueue` `peek` O(1)
**Deque: Double-Ended Queue**
`append_right` `append_left` `pop_left` `pop_right` `peek_left` `peek_right` O(1)
Deque is more powerful than Stack and Queue.
## Python Implementation
```Python
# Stack
stack = [...]
stack.append(e)
stack.pop()
# Queue/Deque
queue = collections.deque([...])
queue.append(e)
queue.appendleft(e)
queue.popleft()
queue.pop()
```
# Monotonic Stack/Deque

> Monotonic Stack 单调栈: 栈内元素顺序永远是单调的，如果入栈新元素会打破这种顺序，那么就先出栈直到新元素加入不会打破这种顺序

For each element of an array, find the closest left/right elements larger/smaller than it in O(N).
	decreasing stack: larger neighbors, increasing stack: smaller neighbors
```Python
# Strictly decreasing stack, left to right traversal example
arr.append(float(inf)) # optional, it makes sure all elements will be poped from stack
stack = [] # For any i, stack[i-1] is the closest left element bigger than stack[i]
pop_index = None
for index, arr_i in enumerate(arr) : 
    while stack and arr[stack[-1]] <= arr_i:
        pop_index = stack.pop()
		# closest left                       closest right
		# arr[stack[-1]] > arr[pop_index] <= arr[index]
    # stack[-1]: closest left element of index > it
	# arr[stack[-1]] > arr[index]
	stack.append(index)
```

```Python
# Solution to the left and right inconsistency in comparison strictness within the while loop
# Use this to substitute `pop_index = stack.pop()`
current = [stack.pop()]
while stack and arr[stack[-1]] == arr[current[0]] : 
    current.append(stack.pop())
for pop_index in current:
```

LC 907:
```python
def sumSubarrayMins(self, arr: List[int]) -> int:
	arr.append(0)
	incstack = [-1]
	ans = 0
	for i, num in enumerate(arr):
		while arr[incstack[-1]] > num:
			popind = incstack.pop()
			# arr[incstack[-1]] <= arr[popind] > arr[i]
			ans += arr[popind] * (popind - incstack[-1]) * (i - popind)
		incstack.append(i)
	return ans % (10**9+7)
```

Lesson: In O(n), for each index we can get left/right closest elements larger/smaller (strict or not) than it
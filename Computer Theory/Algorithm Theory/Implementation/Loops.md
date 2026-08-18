# Correctness: Loop Invariant
**Loop Invariant**: A property that is true before the start of each loop, and true just after the loop ends
Initialization: invariant is true prior to the first iteration of the loop
Maintenance: if the invariant was true, it will be true before the next iteration
Termination: the loop invariant along with the reason the loop terminates gives us a useful property
# Implementations
## Map, Filter & Reduce
### **Map**
```Plain
lst1, lst2
for i in range(lst1.size):
    lst2[i] = func(lst1[i])
lst2 = map(func, lst1)
```
### **Filter**
```Plain
lst1, lst2
k = 0
for i in range(lst1.size){
    if A(lst1[i]):
        lst2[k] = func(lst1[i])
        k++
}
lst2 = filter(func, lst1)
```
### **Reduce**
```Plain
lst
accumulator = initial_value
for e in lst:
    accumulator = func(accumulator, e)
accumulator = reduce(func, lst, initial_value)
```
## Quantifier Logic
if (∀ i in lst, A(i)) do B() [for else logic]
```Plain
for i in lst:
  if (!A(i)):
    break
else:
  B()
```
```Plain
flag = true
for i in lst:
  if !A(i):
    flag = false
    break
if flag:
    B()
```
if (∃ i in lst, A(i)) do B ()
```Plain
for i in lst:
  if A(i):
    B()
    break
```
## While True
**while loop without break:**
B() would run at least once even if A is false at the begining.
```Plain
while A:
    C()
(-------interchangeable-------)
do:
    B()
while A
```
**while true:**
while true has to be used with break so it can halt. break has to be used with if or the code after break is meaningless.
while true plus break is more powerful than while loop without break because it allows both B(code that you want to run at least once) and C(code that you want to run only if A is True) to exist.
```Plain
# These three are equivalent
# 1
while true:
    B()
    if !A:
        break
    C()
# 2 
B()
while A:
    C()
	B()
# 3
while B() and A: # B() returns true
	C()
```
## While Or: consumer of two data structures A & B
```
while A not empty or B not empty:
	if A not empty and C or B is empty:
		# code 1 consumes A
	else:
		# code 2 consumes B
```
## O(N) For-While
It is O(N) because it makes sure each iteration has handled 1 element though not in order
```Python
for left in lst:
		while right is not handled and condition(right):
				handle(right) # right is changed after this
```
Examples:
```Python
# Monotonic Stack
for i in reversed(range(len(nums))):
    while stack and nums[i] >= nums[stack[-1]]:
        stack.pop()
    if stack:
        result[i] = stack[-1] - i
    stack.append(i)
```
```Python
# Sort List Containing 1 to len(nums)
for i in range(len(nums)):
    while nums[i] != i + 1 and nums[nums[i] - 1] != nums[i]:
        nums[nums[i] - 1], nums[i] = nums[i], nums[nums[i] - 1]
```
Also: Sliding Window
# Opinions
1. Once A becomes False in the loop body, it should not be evaluated to True again in any part of the rest of the code.
2. Certain loop conditions must always be True in the loop body.
    
    For this type of loop conditions, every time related variables are changed, we should check the loop conditions again.
    
3. Two ways to implement while loops that are splitted by multiple conditions:
```Python
# 1. Combine all the cases in one loop for progressive change because it is usually clumsy to divide them into cases
# e.g. print number even or odd
i = 0
while i < target:
	print(f"{i} is even")
	i += 1
	if i >= target: return
	print(f"{i} is odd")
	i += 1
# 2. Divided by cases for more unpredictable change, but still we want to avoid this situation
# e.g. two pointers
low, high
while low < high:
	mid = (low + high) // 2
	if answer is after mid:
		low = mid + 1
	else: # answer is mid or is before mid
		high = mid
```
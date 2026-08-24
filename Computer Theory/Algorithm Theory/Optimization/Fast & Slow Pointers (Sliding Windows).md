Core logic of elimination:
```
A(lst[slow:fast+1]) is False, a<=slow<=fast<=b => A(lst[a:b+1]) is False
A(lst[slow:fast+1]) is True, slow<=a<=b<=fast => A(lst[a:b+1]) is True
```
Sample code:
```Python
# Find subarrays lst[slow:fast+1] such that A(lst[slow:fast+1]) is True
# A(lst[0:1]) is True:
slow = 0
for fast in range(len(lst)):
	while not A(lst[slow:fast+1]):
		slow += 1
	ans.append(lst[slow:fast+1])
# A(lst[0:1]) is False: 
slow = 0
for fast in range(len(lst)):
	while A(lst[slow:fast+1]):
		ans.append(lst[slow:fast+1])
		slow += 1
```
When we are trying to find the max length (or more generally, when we know increasing slow would be moot at current fast), we can replace `while` with `if` :
```Python
slow = 0
for fast in range(len(lst)):
	if not A(lst[slow:fast+1]):
		slow += 1
	# A(lst[slow:fast+1]) may still be False, make sure ans won't update in such case
	# If it is about max length, ans won't increase obviously
	ans = max(ans, fast - slow + 1)
```
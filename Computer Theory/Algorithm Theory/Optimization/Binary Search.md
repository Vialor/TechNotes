Binary Search是**双指针算法**淘汰数据情况下的一种特殊情况。特殊之处在于体现了**分治思想中的分割算法**。淘汰的方法是取中点，然后淘汰左右区间中的一个。
有些求值问题看似没有array，也可以强行找到一个范围用Binary Search求解: LC 69, LC 875, LC 410。Quick Select亦用此思路。
# Implementation Tips
## Calculate mid:
用第二种方法而不是第一种来避免mid计算超出整型范围(加法溢出)：
```Python
mid = (low + high) // 2
mid = low + (high - low) // 2
```
## Handle Edge Cases Elegantly:
Note: `len(myList)` 不可为0，否则 `low` 和 `high` 就意义不明了
inclusive lowerbound + inclusive upperbound:
```Python
low, high = 0, len(myList) - 1
while low < high:
	# WHILE LOOP BODY
if low is indeed the solution: return low # 双指针淘汰法的性质导致我们不能确定最后得到的就一定是想要的
else: return -1
```
针对不同问题，WHILE LOOP BODY处有以下几种考虑：
主要取舍的依据在于怎样判断语句怎么好写，因此应该先把判断语句和排除逻辑写完，最后补上mid的定义方式。
```Python
mid = (low + high) // 2 # 这一步导致了后面不能low = mid，否则while loop可能死循环
if 目标在mid:
	low = high = mid
elif 目标仅在mid以前: # 这种情况下一般有low < mid，不然就无解了，所以mid-1不至于超出范围
	high = mid - 1
else 目标仅在mid以后:
	low = mid + 1
```
```Python
mid = (low + high) // 2 # 这一步导致了后面不能low = mid，否则while loop可能死循环
if 目标仅在mid以后:
	low = mid + 1
else 目标在mid及以前:
	high = mid
```
```Python
mid = (low + high + 1) // 2 # 这一步导致了后面不能high = mid，否则while loop可能死循环
if 目标仅在mid以前:
	high = mid - 1
else 目标在mid及以后:
	low = mid
```
Find middle:
```Python
(start + end + 1) // 2: choose mid right when even length
(start + end) // 2: choose mid left when even length
length // 2 is in face the same as (0 + last index + 1) // 2
```
## Python bisect
Binary search on a sorted list: (the leftmost position that num could be in)
```python
bisect.bisect_left(arr, num)
```
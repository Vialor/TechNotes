Sort问题存在以下三个话题：Normal Sorting，Bucket Sorting，Three-Way Partitioning
# Basic Sorting
## O(n^2) Sort
bubble sort/insertion sort/selection sort
bubble sort每一个循环都像鼓泡泡一样，从前到后不断交换相邻元素。list是从后往前建立起来的，每一次循环确定一个元素。
insertion sort每一个循环都是往一个sorted list种insert。list是从前往后建立起来的，每一次循环确定一个元素。
selection sort每一个循环是在select剩下元素中最小的那个。list是从前往后建立起来的，每一次循环确定一个元素。
## O(nlogn) Sort
**Quick Sort**: 简单说就是随机确定一个element为**pivot**，然后把比pivot小的element都放到左边，大的放到右边，然后对左右两边分别递归。(pivot其他的取法：首中尾三个数的中值)
Quick Sort Pivot can be improved by choosing the medium of three random elements.
**Merge Sort**:首先两个长n的sorted list merge成一个sorted list的复杂的O(n), merge_sort做的事情就是把list平分成左右，merge这两个list：merge_sort(left_half)，merge_sort(right_half)
```python
def mergeSort(arr):
	if len(arr) <= 1:
		return arr
	# divide
	a = mergeSort(arr[:len(arr) // 2])
	b = mergeSort(arr[len(arr) // 2:])
	# conquer
	ans = []
	i, j = 0, 0
	while i < len(a) and j < len(b):
		if j == len(b) or i < len(a) and a[i] <= b[j]:
			ans.append(a[i])
			i += 1
		else:
			ans.append(b[j])
			j += 1
	return ans
```
**Heap Sort**: 从左到右，将新遍历的元素插入左侧的PQ中去。最后再依次取出。
  
**取舍：**
Quick Sort:
1. 平均速度非常快，但极端情况下是O(N^2)
2. 额外空间O(logN)，不错
  
Merge Sort:
1. 常常用于linked lists：
时间上的考虑。其它sort的核心操作是swap，基本上都有灵活读取index的需求，而linked list不能很快速的读index，导致速度慢。
空间上的考虑。merge sort核心操作是merge，array会导致我们需要额外的存储空间来放merge后的array，这个问题不存在于linked lists。
2. 具有稳定性：当如果相等的元素并不同质（存在多列数据），Merge Sort可以保留它们原先的相对顺序。
3. 额外空间O(N)，相对糟糕
  
Heap Sort：
1. in place：额外空间O(1)
## Hybrid Sorting Algorithm
**Introspect Sort**
**Tim Sort**
## Quick select
215 quick sort的延伸算法，求list中第几大的数字。要点在于利用pivot将elements分类后，我们现在只需要再递归左右其中一个就行了。
quick sort复杂度位O(nlogn)：n+2*n/2+4*n/4+8*n/8+…+n*n/n
quick select复杂度为O(n)：n+n/2+n/4+n/8+…+n/n
462 求list中位数：即求list中第 `len(nums) // 2` 大的数字
## O(N) Sort
### Bucket Sorting
复杂度为O(n)
bucket sorting的核心在于把要sorting基于的attribute以array的index形式储存。array每一个index都储存着一个array，而这个array的里的element都是具有与该index对应的attribute的。为了构造bucket array，我们还需要先用一个dictionary去收集信息。
比如常见的频率问题 347（attribute就是frequency），我们要用到**两个重要的数据结构：frequencyDictionary和bucketArray**
frequencyDictionary遍历原数据去存储 value:frequency 的键值对，然后bucketArray遍历frequencyDictionary去写frequency:value list
### LSD(least significant digit) Radix Sort
复杂度为O(mN), m是数字位数
Bucket Sort在整数上的应用，从最低位开始sort，共k次
# Three-Way Partitioning
复杂度为O(n)
> 75 Dutch national flag (DNF) problem, 对仅含0，1，2的list排序

其实更像是一个two-pointer的延伸问题，用三个pointer对list进行分类。
```Python
def sortColors(self, nums: List[int]) -> None:
  zeroUpperBound, i, twoLowerBound = 0, 0, len(nums)
  # range(lowerBound, UpperBound)
  while i < twoLowerBound:
    if nums[i] == 0:
      nums[zeroUpperBound], nums[i] = nums[i], nums[zeroUpperBound]
      zeroUpperBound += 1
      i += 1
    elif nums[i] == 1:
      i += 1
    else:
      nums[i], nums[twoLowerBound-1] = nums[twoLowerBound-1], nums[i]
      twoLowerBound -= 1
```
324 Wiggle Sort
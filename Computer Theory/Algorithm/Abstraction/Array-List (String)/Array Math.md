> In O(N), we can find: min/max, sum/pre sum, k extremals/median, average, mode
# Find k extremals from N (Median)
1. Quick Select: O(N)
LC 462 求list中位数：即求list中第 `len(nums) // 2` 大的数字
LC 215 kth largest
```Python
def findKthSmall(k):
    low, high = 0, len(C) - 1
    while True:
        pivot = random.randint(low, high)
        C[pivot], C[low] = C[low], C[pivot]
        pivot, splitPoint = low, high
        while pivot < splitPoint:
            if C[pivot] >= C[pivot + 1]:
                C[pivot], C[pivot + 1] = C[pivot + 1], C[pivot]
                pivot += 1
            else:
                C[splitPoint], C[pivot + 1] = C[pivot + 1], C[splitPoint]
                splitPoint -= 1
        if pivot > k:
            high = pivot - 1
        elif pivot < k:
            low = pivot + 1
        else:
          return C[pivot]
```
other partition methods
```
def hoare_partition(arr, low, high):
    pivot = arr[low]  # 选择第一个元素作为枢轴
    left = low - 1
    right = high + 1

    while True:
        # 左指针向右移动，直到找到大于等于枢轴的元素
        left += 1
        while arr[left] < pivot:
            left += 1

        # 右指针向左移动，直到找到小于等于枢轴的元素
        right -= 1
        while arr[right] > pivot:
            right -= 1

        # 如果左右指针交错，分区完成
        if left >= right:
            return right

        # 交换左右指针所指的元素
        arr[left], arr[right] = arr[right], arr[left]

def lomuto_partition(arr, low, high):
    pivot = arr[high]  # 选择最后一个元素作为枢轴
    i = low        # i 用于标记小于等于枢轴的区域

    for j in range(low, high):
        if arr[j] <= pivot:
            arr[i], arr[j] = arr[j], arr[i]  # 交换元素，保证小于等于枢轴的元素在左侧
            i += 1

    # 将枢轴元素放到正确的位置
    arr[i], arr[high] = arr[high], arr[i]
    return i  # 返回枢轴的位置
```
Hoare is usually faster, but it didn't enforce the positions of elements that equals the pivot like Lomuto. 
2. Heap: O(Nlogk)
if k<<N, heap might be better in practice
# Mode
## Boyer-Moore Majority Vote Algorithm
LC 169 求list中过半的元素（前提已知存在过半元素）
```Python
candidate: 当前候选人, 只有一个考虑名额。
vote: 当前候选人计票，如果vote不大于0，则考虑下一个候选人。
num in nums: 遍历每个num，如果支持当前候选人则投一票，否则减一票，一个num只有一次机会。
```
最恶劣的情况下，所有不是majority的人都投了majority的反票。  
（因为，他们还可能浪费票数反对彼此）  
即便如此，majority还是应该胜出，因为人数更多。  
## General Solution
```Python
countDict = defaultdict()
modeFrequency = 0
mode = None
for num in nums:
	countDict[num] = countDict[num] += 1
	if modeFrequency < countDict[num]:
		modeFrequency = countDict[num]
		mode = num
```
# Pre-Sums
`preSums[i] = sum of array[0] to array[i-1]` (sum before index i)
`sum of index i to j = preSums[j+1] - preSume[i]`
`sum(array) = preSums[-1] = preSums[len(array)]`
```Python
preSums = [0]
for num in array:
	preSums.append(preSums[-1] + num)
```
**Benefits**: store N^2 information in N space with N time complexity
# Max Subarray Sum - Kadane's Algorithm
LC 918
```python
def kadane(nums: List[int]) -> int:
	n = len(nums)
	maxSum = float("-inf")
	maxCurEnding = 0
	for i, num in enumerate(nums):
		maxCurEnding += num
		maxSum = max(maxCurEnding, maxSum)
		maxCurEnding = max(0, maxCurEnding)
	return maxSum
```
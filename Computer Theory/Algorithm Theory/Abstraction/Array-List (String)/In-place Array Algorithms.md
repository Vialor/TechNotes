reverse
```Python
def reverse(self, arr: List[int]) -> None:
	for i in range(len(arr) // 2):
		arr[i], arr[-i] = arr[-i], arr[i]
```
rotate
```Python
def rotate(self, nums: List[int], k: int) -> None:
		def reverse(start, end):
		    while start < end:
		        nums[start], nums[end] = nums[end], nums[start]
		        start += 1
		        end -= 1
		
		N = len(nums)
		k %= N
		reverse(0, N-1)
		reverse(0, k-1)
		reverse(k, N-1)
```
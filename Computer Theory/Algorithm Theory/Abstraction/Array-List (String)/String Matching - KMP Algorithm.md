**KMP Knuth–Morris–Pratt Algorithm**: Find the first occurrence of needle string in haystack string
The key is **next array**:
1. What does next array do: When `haystack[i] != needle[j]`, the next to compare is `haystack[i]` and `needle[nextArray[j-1]]`
2. How to calc next array: `next[j]` = the length of longest equal prefix and suffix of `needle[:j+1]` (prefix and suffix cannot be the same)
## Build Next Array
> Complexity O(len(s))

LC 1392
```python
def buildNextArray(self, s: str) -> str:
	# nextArray[i]: the length of longest equal prefix and suffix of s[:i]
	nextArray = [0]
	for i in range(1, len(s)):
		j = nextArray[i-1] # prefix len
		while j > 0 and s[j] != s[i]:
			j = nextArray[j-1]
		if s[j] == s[i]:
			j += 1
        nextArray.append(j)
	return nextArray
```
## Main function 
#TODO
```python
def KMP(haystack, needle):
	nextArray = buildNextArray(needle)
	i, j = 0, 0
	while i < len(haystack):
		if haystack[i] == needle[j]:
			i += 1
			j += 1
		elif j > 0:
			j = nextArray[j-1]
		else:
```
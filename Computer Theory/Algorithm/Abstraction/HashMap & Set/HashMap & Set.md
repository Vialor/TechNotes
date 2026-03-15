**Power**
HashMap is more powerful than Set. Effectively, set is a HashMap mapping keys to Booleans.
HashMap is more powerful than Array. Effectively, array is a HashMap whose keys are natural numbers.
**Implementation**
[[Hashing Under The Hood]]
Under the hood, set is a hash table using the element itself both as key and as value. When you check if an element is in the set, it checks whether the element exists at the location determined by its hash.
# Algorithm
### Counter
Count elements of an array.
```python
# 1
frequencyCounter = collections.Counter(array)
# 2
frequencyCounter = {}
for elem in array:
	if elem in frequencyCounter:
		frequencyCounter[elem] += 1
	else:
		frequencyCounter[elem] = 1
```
### Back Tracer (Isomorphism)
Trace back to any data structure object by its content(value).
Warning: Assume no duplicates, or duplicates can be ignored
Examples:
1. Trace back to an array’s index by its element while traversing.
Duplicates can be ignored in LC 1.
Elements in PreSums of arrays of positives are unique naturally like LC 560.
```Python
indexTracer = {elem:index for index, elem in enumerate(arr)}
```
2. Trace back to an Graph Node by its content(value) while traversing.
LC 133
### Dynamic Programming Caching
(an example that array is less powerful than hash map)
Functions: mapping input parameters to an output to avoid recalculation.
Pointer based structure: mapping object address to avoid recalculation, which leads to the next usage. (This has the same effect as adding new field to the object)
### DFS Visiting Information
Usually, set is enough, but hey check out LC 138, it stores DFS visiting info and also serves as a back tracer to cloned object.
List Multiplication:
```python
def lst_mult(self, l1, l2):
	return [l1[i] for i in l2]
```
**Discretization** of a list: translate a list of elements to order 0,1,2... preserving the internal relative order
Complexity: O(nlogn)
```Python
indexBook = sorted(set(originalList)) # index:elem # set can be removed if no dups
elemBook = {elem:i for i, elem in enumerate(indexBook)} # elem:index
discretizedList = [elemBook[elem] for elem in originalList]
```
**Permutation** is a special orthogonal matrix
```Python
lst # ['a','b','c']
elemBook = {elem:i for i, elem in enumerate(lst)} # {'a':0,'b':1,'c':2}

pList = permute(lst) # ['b','c','a]
permutation = [elemBook[elem] for elem in pList] # [1,2,0]

[lst[i] for i in permutation] # pList ['b','c','a]
```



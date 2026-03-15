a items with b potential states, the total of different information is b<sup>a</sup>
# String Encoding
## Non-linear Encoding: value range of info
> Certain value ranges are more important than others

**Logistic Compression** on voice data: human ears are less sensitive to sound of high Hz
## Delta Encoding: relevancy of info
> Data changes gradually instead of drastically over time
## Variable-length Code: frequency of info
Redundancy = weighted average code length - entropy
### Huffman Code
>A variable-length encoding, where less frequent element is assigned with longer encoding. 

To avoid ambiguity, no encoding should be the prefix of another encoding. In a binary tree representation, all the encodings are at the leaf nodes.
#### Implementation: greedy algorithm
```python
# freq: a list of tuples: [(frequency, element tree node)]
def huffman_code(freq):
	n = len(freq)
	heapq.heapify(freq)
	for _ in range(n-1):
		z = TreeNode()
		lfreq, z.left = heapq.heappop(freq)
		rfreq, z.right = heapq.heappop(freq)
		heapq.heappush(freq, lfreq + rfreq, z)
	return freq[0]
```
# Matrix Encoding

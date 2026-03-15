## Slicing: inclusive upper bound & exclusive upper bound
Justification of the exclusive upper bound
```Plain
j is exclusive:
length: j-i
the middle index: (i+j) // 2
j is inclusive:
length: j-i+1
the middle index: 
(i+j+1) // 2 # mid can equal j, prefered
(i+j) // 2 # mid can equal i
```
## nature of array subset
Subsequence ⇒ choose or not
Subarray(contiguous) ⇒ two pointers

**Use array as a cycle:** `arr[i % len(arr)]`

commutative law: A . B = B . A
associative law: (A . B) . C = A . (B . C)
identity law: exists I s.t. A . I = I . A = A
# Matrix Operations

点乘 element-wise multiplication $C_{ij} = A_{ij} * B{ij}$
```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
C = A * B
```
叉乘 cross product
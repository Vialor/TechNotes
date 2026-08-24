D&C is a three-step solution: **divide → conquer → combine**
# Motivation
1. D&C is an optimization on time complexity:
Time Complexity: T(n) = k\*T(n/k) + divide and combine complexity
as long as combine complexity is within O(n), the overall complexity is O(nlogn)
**Why does this save time?**
Because it avoids comparing a and c by this logic: a > b, b > c ⇒ a > c. This happens when **merging/combining**.
2. D&C is a necessity on space complexity:
Data taking up too much space, and we need to store it separately, say in different servers.
# Master Theorem
If `a ≥ 1` and `b > 1` are constants, and and `f(n)` is an asymptotically positive function, then the time complexity of a recursive relation is given by T(n) = aT(n/b) + f(n) where, T(n) has the following three asymptotic bounds:
## Dumb Version
$f(n) = n^d$:
1. Cost dominated by leaves: If $log_ba>d$, then $T(n)=\Theta(n^{log_ba})$
2. Cost dominated in balance: If $log_ba=d$, then $T(n)=\Theta(n^d * log n)$
3. Cost dominated by root: If $log_ba<d$, then $T(n)=\Theta(n^d)$
### Proof
Depth of recursion: $log_bn$
Number of sub-problems in layer i: $a^i$
Size of a sub-problem in layer i: $n\over b^i$
The work in layer i: $a^i({n\over b^i})^d=({a\over b^d})^in^d$ #TODO
## Complete Version
1. Cost dominated by leaves: If $f(n)=O(n^{log_ba-\epsilon})$ for constant $\epsilon>0$, then $T(n) = \Theta(n^{log_ba})$
2. Cost dominated in balance: If $f(n)=\Theta(n^{log_ba}log^kn)$ for constant $k>0$, then $T(n) = \Theta(n^{log_ba}log^{k+1}n)$
3. Cost dominated by root: If $f(n)=\Omega(n^{log_ba+\epsilon})$ for constant $\epsilon>0$ f satisfies the *regularity condition*, then $T(n) = \Theta(f(n))$
	*regularity condition*: af(n/b) <= cf(n) for some constant c < 1 and sufficiently large n
# Classic Algorithms
## Strassen's Matrix Multiplication
$$
\begin{bmatrix} x11 & x12 \\ x21 & x22 \end{bmatrix}
\begin{bmatrix} y11 & y12 \\ y21 & y22 \end{bmatrix}
=?
$$
$T(n) = 7T(n/2)+\Theta(n^2)=\Theta(n^{log_27}）$
Strassen's Matrix Multiplication manages to reduce from 8 to 7
## Karatsuba’s Divide and Conquer Algorithm: Integer Multiplication
Suppose int ab divided into halves a and b; cd divided into halves c and d
$ab*cd = shift2(a*c) + shift1(b*c + a*d) + b*d$
Note that $(b*c + a*d) = (a+b)*(c+d) - a*c - b*d$
So we need only 3 multiplications, and the complexity is:$T(n) = 3T(n/2)+\Theta(n)=\Theta(n^{log_23})$
## Closest pair of points in the plane
Complexity: $T(n) = 2T(n/2) + O(n) = O(nlogn)$
```
// Px and Py have the same elements, Px is sorted by x, Py is sorted by y
CLOSEST_PAIR_REC(Px, Py):
	if |Px| <= 3: resolve and return
	Construct Qx, Qy from left half of Px
	Construct Rx, Ry from right half of Px
	q1, q2 = CLOSEST_PAIR_REC(Qx, Qy)
	r1, r2 = CLOSEST_PAIR_REC(Rx, Ry)
	δ = min{ distance(q1, q2), distance(r1, r2) }
	xmid = max x-coordinate in Qx
	Construct Y = (s1, s2, ..., sm) where si = (xi, yi) from Py and xi is within the δ distance of xmid
	for i = 1 to m:
		for j = i+1 to min(i+7,m):
			if d(si, sj) < δ:
				δ = distance(si, sj)
	return the corresponding points for δ
```
## City Skyline
Leetcode 216
```Python
def getSkyline(self, buildings: List[List[int]]) -> List[List[int]]:
	def DnCcombine(skyline1, skyline2):
		height1 = height2 = 0
		i1 = i2 = 0
		ans = []
		while i1 < len(skyline1) and i2 < len(skyline2):
			i = skyline1[i1][0]
			if skyline1[i1][0] < skyline2[i2][0]:
				height1 = skyline1[i1][1]
				i1 += 1
			elif skyline1[i1][0] > skyline2[i2][0]:
				i = skyline2[i2][0]
				height2 = skyline2[i2][1]
				i2 += 1
			else:
				height1, height2 = skyline1[i1][1], skyline2[i2][1]
				i1 += 1
				i2 += 1
			h = max(height1, height2)
			if len(ans) == 0 or ans[-1][1] != h: ans.append([i, h])
		ans.extend(skyline1[i1:] or skyline2[i2:])
		return ans

	def DnC(bstart, bend): # bend is exclusive
		if bstart >= bend:
			return []
		elif bstart + 1 == bend:
			b = buildings[bstart]
			return [[b[0], b[2]], [b[1], 0]]
		else:
			mid = (bstart + bend) // 2
			return DnCcombine(DnC(bstart, mid), DnC(mid, bend))
	return DnC(0, len(buildings))
```


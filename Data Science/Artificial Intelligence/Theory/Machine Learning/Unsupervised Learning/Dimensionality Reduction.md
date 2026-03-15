## SVD Singular Value Decomposition
> The first dimension is the direction in which the points exhibit the greatest variance, the second dimension the second greatest, and so on, until we have enough dimensions that variance becomes really low

==Example: Reduce m\*n matrix A==
$A \approx USV^Τ$
A: input data matrix, m\*n
U: left singular vectors, m\*r; V: right singular vectors, n\*r;
	U, V are column orthonormal
S: singular values, r\*r
	S is diagonal, containing variance on the column axes of V
V: right singular vectors, n\*r
r: rank
$AV = USV^TV = US$: projected data onto V

Dimension reduction is done by removing the smallest singular values in Σ and corresponding last columns in U and V
### SVD Implementation
If A is symmetric, $A = USU^T, S = U^TAU$

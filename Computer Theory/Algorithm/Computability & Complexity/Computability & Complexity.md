# Computability Theory
## Problem transformation
$A\leq_p B$: **Problem A is polynomial time reduceable to B** (Intuitively, A is no harder than B)
There exists a polynomial time computable function f, such that x solves A <=> f(x) solves B
f is called **polynomial time reduction of A to B**

*Decision problem* (yes-or-no problem) and *Optimization problem* are interchangeable and the same hard to solve.
	A optimization problem can be changed to a binary search with many decision problems
## Computability
**P Polynomial Time**: a solution to this problem can be found in polynomial time
**NP Nondeterministic Polynomial Time**: a solution to this problem can be found in nondeterministic polynomial time; in other words, a solution to this problem can be verified in deterministic polynomial time
	==P is a subset of NP, but P = NP is not proved yet==
**NP-hard**:  every NP problem has a polynomial-time reduction to it
	NP-hard is at least as hard as NP
**NPC (NP-complete)**: Intersection of NP-hard and NP
**NPI (NP intermediate)**: NP, but neither P nor NP-hard 
## [[NPC Problems]]
# Complexity
## Asymptotic Notations
f(n) is O(g(n)) if there exists a c > 0 and an n0 such that f(n) ≤ cg(n) for all n ≥ n0
	f(n) is o(g(n)) if there exists a c > 0 and an n0 such that f(n) < cg(n) for all n ≥ n0
f(n) is Ω(g(n)) if there exists a c > 0 and an n0 such that f(n) ≥ cg(n) for all n ≥ n0
	f(n) is ω(g(n)) if there exists a c > 0 and an n0 such that f(n) > cg(n) for all n ≥ n0
$\Theta$: Ω and O

$\Theta(1)<\Theta(lg(lgn))<\Theta(lgn)<(lgn)^2<\Theta(\sqrt n)<\Theta(n)<\Theta(n^2)<\Theta(2^n)<\Theta(n!)$

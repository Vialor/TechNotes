> Goal: O(1) insert, delete and search

Naive Solution - **Direct addressing**: Reserve a slot for each possible key
	need a lot of space
	
**Hash Table**: an array T of size m
**Hash Function** h: U->{0,...,m-1}
	$k\in U$ hashes in slot h(k) in T
# Solve Collision
## Open addressing
Linear Probing & Quadratic Probing
Python: Perturbation Probing
## Chaining
Java: When linked list reaches length 8, Java will use red black tree instead
## Rehashing
Java: rehash when too many collisions
# Universal Hashing
>When hashing, choose a hash function h *randomly* from $H={h_1,h_2...h_w}$
	*randomly*: independently from the keys to be stored

H is **universal** if $\forall k,k'\in U,Pr_{h\in H}(h(k)=h(k'))\leq {1\over m}$, where m is the size of the table
	i.e. functions in H are good enough that the expected chance to collide given two different keys is no more than 1/m
**Universal Family**: $H_{p,m}=\{h_{a,b}(k)=((ak+b)\mod p)\mod m\}$, where $0<a\leq p-1$, $0\leq b\leq p-1$ and p is a prime number larger than all possible keys
	Restrictions on p ensure that any possible collisions should happen after mod m not p
# Static Perfect Hashing
>Create a no-collision hashing for static keys

A 2-level hashing scheme
	First level: Use universal hashing to distribute keys across the first-level.
		randomly choosing hash function until $\Sigma_{j=0}^{n-1} n_j \leq 4n$
		N.B. $P(\Sigma_{j=0}^{n-1} n_j \leq 4n) \geq 1/2$
	Second level: for each index, use another universal hash function to perfectly hash, each index with $n_j$ colliding keys has a secondary hash table of size $n_j^2$
		randomly choosing hash function until no collision
		N.B. $P(X<1) \geq 1/2$, where X is the number of collisions
# Bloom Filter
> **Bloom Filter**: An array of Boolean values that supports: 1. store item, 2. verify if item exists

Use case: efficiency in space and time at the cost of *accuracy*
	*accuracy*: can have false positives, though impossible to have false negatives
	e.g. filter spam emails
## Simple implementation
```Python
def bf_store(bf, item):
	for h in H:
		bf[h(item)] = 1

def bf_exists(bf, item):
	for h in H:
		if bf[h(item)] == 0:
			return False
	return True
```
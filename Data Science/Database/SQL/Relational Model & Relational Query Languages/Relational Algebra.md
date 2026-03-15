# Basic Operations
**Single relation operations:**
	Select (Selection of rows): $\sigma_{attribute\_constraints}(relation)$﻿
	Project (Selection of columns): $\pi_{attributes}(relation)$﻿
	Rename (Rename attributes A1, A2, A3… as a whole to x): $\rho_{x(A1, A2, A3...)}(relation)$﻿
	Assignment (Assign a relation to R): R ← relation
**Inter-relations operations (for relation r and relation s):**
	Union: $r\cup s$﻿; Intersection: $r\cap s$﻿; Difference: $r-s$﻿
	Cartesian Product: $r \times s$﻿; Division: $r\div s$﻿
	Natural Join: $r\bowtie s$﻿; ⟕ natural left outer join; Theta Join: $r\bowtie_\theta s = \sigma_\theta(r\times s)$﻿; Left/Right/Full Outer Join

> Know-hows:  
> Universal quantifier is difficult, and exsistential quantifier is easy. The former usually acheived by finding the complement set.  
# Extended Relational Algebra Operations
**Generalized projection**: $\pi_{f1(K1),f2(K2)...}(relation)$﻿  
fi:  
_takes in each value of a set of attributes_
**Aggregate Functions**: $_{G1, G2...Gn}\gamma_{f1(A1)as\space name1,f2(A2)as\space name2...fn(An) as\space name n}(relation)$﻿  
Gi: a list of attributes on which to group  
fi: aggragate function, which  
_takes in all the values of an attribute_ (avg, min, max, sum, count)  
Ai: columns
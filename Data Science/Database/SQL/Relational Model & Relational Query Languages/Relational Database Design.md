**Goal of Relational Database Design**: Use decomposition to make sure each relation schema is in normal form. In this process, should make sure **lossless-join**, and preferably **dependency preserving**
	**lossless-join**: after decomposition, there is no information loss
	**dependency preserving**: after decomposition, it is possible to enforce all the functional dependencies just by enforcing them on the individual tables of the decomposition, without needing to join the tables back together
		The motivation of dependency preserving is to simplify constraint maintenance
# Functional Dependency Theory
**functional dependency fd**: Given $\alpha \subseteq R$﻿ and $\beta \subseteq R$﻿, α → β holds on R if and only if for any legal relations r(R), for any two tuples t1 and t2 of r agree on the a set of attributes α, they also agree on the attributes β. ($t1[\alpha] = t2 [\alpha] \rightarrow t1[\beta] = t2 [\beta]$)

K is a **superkey** for relation schema R iff K→R
K is a **candidate key ck** for R iff K→R, and there is no $\alpha \subseteq K$ , α→R

α → β is **trivial** if $\beta \subseteq \alpha$

Given a set F of functional dependencies, the set of all functional dependencies logically  
implied by F is the  **closure of F denoted by F+**.
## Canonical Cover
**Extraneous Attributes:** Consider a set F of functional dependencies and the functional dependency α→β in F:
	Attribute A is **extraneous in α** if $A\in \alpha$﻿ and F logically implies $(F–\{α→β\})\cup\{(α–A)→β\}$. 
	Attribute A is **extraneous in β** if $A\in \beta$﻿ and $(F–\{α→β\})\cup\{α→(β-A)\}$ logically implies F.  
A **canonical cover** for F is a set of dependencies **Fc** such that
- F and Fc  are logically equivalent
- No functional dependency in Fc contains an extraneous attribute
- Each left side of functional dependency in Fc is unique.
## Lossless-join & Dependency Preserving
Decomposing R into R1 and R2:
A *sufficient* condition for lossless-join: at least one of $R_1 \cap R_2 \rightarrow R_1$﻿ and $R_1 \cap R_2 \rightarrow R_2$ is in F+
	This condition becomes *necessary* from *sufficient* when all constraints are functional dependencies
Dependency Preserving: if Fi has and only has attributes of Ri then $(F_1\cup F_2)^+=F^+$
# Normal Forms
## First Normal Form
Domain is **atomic** if its elements are considered to be indivisible units
A relational schema R is in **first normal form** if the domains of all attributes of R are atomic
Exceptions to not to follow first normal form: time, address
## BCNF Boyce-Codd Normal Form
> There is always a lossless-join, but maybe not dependency preserving decomposition into BCNF

A relation schema R is in BCNF with respect to a set F of functional dependencies iff for all functional dependencies in F+ of the form α→β are either trivial or α is a superkey for R
### Join-lossless Decomposition into BCNF
For a schema R and a non-trivial dependency a → b:
We decompose R into: (a U b) and (R - ( b - a ))
## 3NF Third Normal Form
> Relax BCNF to ensure **dependency preserving** during the decomposition
> There is always a lossless-join and dependency preserving decomposition into 3NF
> BCNF is a subset of 3NF

A relation schema R is in Third Normal Form with respect to a set F of functional dependencies:
iff for all functional dependencies in F+ of the form α→β are at least one of the following:
1. trivial
2. α is a superkey for R
3. each attribute in β-α is contained in a candidate key for R﻿
### Join-lossless Decomposition into 3NF
Create a relation for every fd in the canonical cover.
Then create a relation for every ck not already included in the prev step.
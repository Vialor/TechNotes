> cannot express aggregation functions
# Relational Calculus
## Tuple Relational Calculus TRC

> {t|P(t)}: returns tuples where P(t) is true, P is similar to a _predicate calculus formula  
> Note: $\exists t\in r(Q(t))$_﻿ there exists” a tuple in t in relation r such that predicate Q (t ) is true
### Safety of Expression
**Why:**
It is possible to write tuple calculus expressions that generate infinite relations  
For example, { t | ¬ t | r } results in an infinite relation if the domain of any attribute of relation r is infinite  
To guard against the problem, we restrict the set of allowable expressions to safe expressions.  

**What:**  
An expression {t | P(t)} in the tuple relational calculus is **safe** if every component of t appears in one of the relations, tuples, or constants that appear in P.  
This is more than just a syntax cndition, for example  
$\{ t | t [A] = 5 \lor true \}$﻿ is not safe
### Examples
1. Find the names of all instructors whose department is in the Watson building$\{t|\exists s\in instructor(t[name]=s[name]\land \exists u\in department(u[deptname]=s[deptname]\land u[building]="Watson"))\}$
1. Find all students who have taken all courses offered in the Biology department$\{t|\exists r\in student(t[ID]=r[ID])\land(\forall u\in course[deptname]="Biology" \rightarrow \exists s\in takes(t[ID]=s[ID]\land s[courseid]=u[courseid]))\}$
## Domain Relational Calculus DRC

> {<x1, x2, …, xn> | P(x1, x2, …, xn)}: x1, x2,.., xn are domain variables, P is similar to a _predicate calculus formula_

> DRC is (roughly) the basis for **Query By Example QBE**, a graphical query language
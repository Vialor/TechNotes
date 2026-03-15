Sample Space S; Outcome $s\in S$﻿  
  
**Random Variable RV** X(s) = n: X is a function mapping outcome s to a number n
Events: ranges of values of random variables  
  
**Probability Function P**: a function mapping events to [0, 1]  
Sometimes for simplicity, P(X=x) is written as P(x)  
**Inclusion-Exclusion Principle**:$P(\bigcup\limits_{i=1}^{n} A_i)=\sum\limits_{\varnothing\neq J\subseteq\{1...n\}}(-1)^{|J|+1}(P(\bigcap\limits_{j\in J}A_{j}))$
# Expected Value
For discrete RV X: $E(X)=\sum\limits_x xP(x)$﻿
For continuous RV X: $E(X)=\int^{\infty}_{-\infty}xf_X(x)dx$﻿
Math properties:
$E(h(x))=\sum h(x)P(x)$﻿  
  
$E(h(x))=\int^{\infty}_{-\infty}h(x)f_X(x)dx$﻿
Hence, $E(a+bX)=a+b(E(x))$

Markov's Inequality: $P(X\geq t)\leq {E(X)\over t}$, X is non-negative
# Conditional Probability & Independence
$P(A|B)P(B)=P(A\cap B)=P(B|A)P(A)$﻿  
  
**Bayes’ Rule**: $P(A|B)={P(B|A)P(A)\over P(B)}$﻿
A and B are **independent** with respect to probability function P when $P(A\cap B)=P(A)P(B)$﻿

**Mutual Independence**: for a finite collection of events, any subset J has $P(\bigcap\limits_{i\in J} A_i)=\prod\limits_{k\in J}P(A_i)$﻿  
If any two-element subset has this property, it is called  
**pairwise independence**.
A more general formula is **Multiplication Rule of Probability**: $P(\bigcap\limits_{i=1}^n A_i)=\prod\limits_{k=1}^nP(A_k|A_{k-1},...,A_1)$
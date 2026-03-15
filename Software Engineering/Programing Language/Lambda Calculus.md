# Expression
```Python
<expression> := <variable>
	|λ<variable>.<expression>
	|<expression><expression>
	|(<expression>)
```
f = λxy.(x+y) means f(x, y) = x+y, x and y are **bound variables** here
f = λ.x+1 means f()=x+1 or x+1, x is a **free variable** here
## Calculation
x/y means to replace y with x
left associative: M N P = ((M N) P)
**α-substitution**: rename variables when the same variable name appears in different lambda bindings
λx.E[x] = λy.E[y/x]
**β-reduction**: apply the arguments to the lambda expression
(λx.M[x])N = M[N/x]
### Currying
(λx.λy.(x+y))a = (λx.(λy.(x+y)))a = λy.(a+y)
### Normal form
**Normal form** E*: after several reductions, E cannot be rewrited again
**Church-Rosser Theorem**: Different evaluation orders, if exist, always result in the same normal form
# Representation of other concepts
Booleans: T=λxy.x; F=λxy.y  
Logic: AND=λxy.xyF; OR=λxy.xTy; NOT=λx.xFT  
Church’s number: 0 = λsz.z; 1 = λsz.s(z); 2 = λsz.s(s(z))  
Successsor/Addition: S = λnab.a(nab); example: 2S3  
Multiplication: (λxyz.x(yz))  
Pairs: pair=λz.zab; pair=λab.λf((f a) b)  
first=T, second=F  
list h t=pair h t, t is a list
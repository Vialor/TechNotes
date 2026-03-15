# Basic Grammar
import `include("test.jl")`
## String
```julia
str = "Hello World"
str[begin] # 'H'
str[1] # 'H'
str[end] # 'd'
str[begin:5] # "Hello"
```
## Function
```julia
function fact1(n)
  fact = 1
  for i in 1:n
    fact = fact*i
  end
  fact
end

fact2(n) = n == 0 ? 1 : n*my_fact(n-1)

function counter()
	call_times = 0
	function increment()
		call_times += 1
	end
end
```
## Type System
types `var :: <type>`; subtypes `var <: <type>`
**concrete types**: ~ represent specific, concrete instances of objects in memory; ~ are final and may only have abstract types as their supertypes
	**primitive type**: a concrete type that is not defined with struct
**abstract types**: ~ represent collections of related concrete types; ~ cannot be instantiated directly
```julia
# Concrete type
struct Point
	x::Float64
	y::Float64
end

p = Point(3.5, 4.2)
p.x # 3.5

# Abstract type
abstract type Shape end

# Concrete subtype of Shape
struct Circle <: Shape
	center::Point
	radius::Float64
end
```
> Consider the type hierarchy as a tree. Concrete types would be the leaf nodes, abstract types the non-leaf nodes 

In Julia, a **method** is a specific implementation of a **function** for particular argument amount and particular argument types.
**Method dispatch** is the process of selecting the appropriate method to call when a function is invoked.
	Method Ambiguities: errors out when more than one methods match the current typing

**Parametric types**:
```julia
# 1
struct Point{T}
	x::T
	y::T
end

Point{Float64} <: Point # true
Point{Float64} <: Point{Real} # false

# 2
same_type_numeric(x::T, y::T) where {T<:Number} = true
same_type_numeric(x::Number, y::Number) = false
```
# Metaprogramming
## Macro & Quoted Expression
`:(x + y)` and `quote x+y end` are the same; the former exits upon `)`, so use quote for this case
`$` is the Interpolation
```julia
macro inception(f, ex)
  f, ex = esc(f), esc(ex) # important hygiene practice
  return quote
    x = $ex
    y = $f(x)
    z = $f(y)
    x, y, z
  end
end

@inception sin 1
# for details
@macroexpand(@inception sin 1)
```
## AST & S-expression
**Abstract Syntax Tree AST**: the structure of a piece of code in a hierarchical format; it is used by compilers and interpreters. In Julia AST is represented by **s-expression**

`parse(str)`, `show_sexpr(prog)`, `eval(prog)` & Expr
```julia
x = 1
str = "1 + $x"
# string => quoted expression
prog = Meta.parse(str) # :(1 + 1)
# quoted expression => print s-expression
ast = Meta.show_sexpr(prog) # (:call, :+, 1, 1)

Expr(:call, :+, 1, 1) # :(1 + 1)
Meta.eval(prog) # 2
```
## Others
```julia
fieldnames(Point) # (:x, :y)

dump(Point(3.1,4.2))
# Point{Float64}
#   x: Float64 3.1
#   y: Float64 4.2
```

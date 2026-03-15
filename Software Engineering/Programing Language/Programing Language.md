[[Devtools]]
Metaprograming: computer programs have the ability to treat other programs as their data
**Programming Paradigms**: Imperative vs Declarative
Abstraction:
	**Abstraction by parameterization**: defining a parameters representing a generalized solution, typically in FP
	**Abstraction by specification**: defining an interface or abstract class representing a generalized solution, typically in OOP
# Imperative Programming Languages: Fortran family
**How to do**
## Scripting Languages

> dynamic typing, automatic Memory Management, run by interpreters (no compilation)

[[JS]]
[[Python]]
## Object-Oriented Programming OOP
**Encapsulation**: data and methods are encapsulated within an object or class
**Inheritance**: an object can inherit from other objects
**Abstraction**: interface which defines a templated to be implemented by classes
**Polymorphism**:  
    **runtime/dynamic polymorphism**: Through **Method Overriding**  
    A subclass provides a specific implementation of a method that is already defined in its superclass. The decision about which method implementation to execute is made at runtime based on the actual object's class  
    **compile-time/static polymorphism**: Through **Method Overloading:**  
    Multiple methods in the same class to have the same name but differ in the types or numbers of parameters. The decision about which method to call is made at compile-time based on the method signature.  
[[OOP Design Patterns]]

[[C ⇒ C++]] (C is not OOP, C++ is)
[[Java]]
# Declarative Programming Languages: Lisp family
**What to do**
## Markup Languages
HTML CSS
## Functional Programming FP
**First-class function**: function can be assigned and returned like any other values
**Pure function**: 1. Referential transparency: same input always leads to same output 2. no side effects, including IO and global var changes  
Which also means that there will be no statements only expressions  
**Lazy evaluation**: call by name, arguments are only evaluated when necessary
**Immutability**: all data are immutable, always create a new data instead of modifying the original one
[[Lambda Calculus]]
[[Higher Order Functions]]

[[Lisp]]
[[Haskell]]
## Logic Programming
Programming languages based on propositional logic
[[Prolog]]
# Multi-Paradigm Programming Languages
[[Julia]]
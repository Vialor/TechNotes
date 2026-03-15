> Compiler: SBCL Steel Bank Common Lisp

Data Types: atom: A; list: (A, B, C)  
atom a: wether a is an atom  
**List Operations:**
lists are built with **cons cells**: (cons 1 (cons 2 (cons 3 (cons 4 (cons 5 '()))))) ⇒ (1 2 3 4 5)  
user  
`.` as a shorthand for cons: (cons 1 nil) ⇒ (1 . nil)  
a proper list ends with nil, (1 2 3 4 5) has a hidden nil  
car ‘(A, B, C): returns the first element A  
cdr ‘(A, B, C): returns the list from second element (B, C)  
member 3 (1,2,3,4,5): check if 3 is in the list  
null ‘(A, B, C): check if empty  
nth 2 ‘(A, B, C, D): returns the element at index 2, C  
nthcdr 2 ‘(A, B, C, D): returns the list from index 2, (C, D)  
**Booleans:**  
True: T or t; False: Nil or ()  
**Conditional Statements:**
```Lisp
IF (condition) (then_clause) (else_clause)
COND ((condition1)(function1)) ((condition2)(function2)) …
```

| CBV call by value                                              | CBN call by name                                          |
| -------------------------------------------------------------- | --------------------------------------------------------- |
| evaluate arguments before β-substitution/applying the function | evaluate only when necessary(after applying the function) |
| not always terminate                                           | always terminate                                          |
| functions                                                      | macros                                                    |
**Binding:** SETQ SETF  
  
`(setq a 1 b 2 c 3)` binding multiple vars  
SETF is more powerful as it allows for more complex operations like  
`(setf (car list) 100)`
**Define**: (Lisp is stateful)  
  
`defparameter x 1`
# Clarification: List data or Program
lists are both data and programs. Use `'` to indicate data instead of program, so ‘(A B C) is a literal list, (A B C) is applying A to B & C
`` ` `` is similar to `'`, yet variables after `,` in the template are evaluated: `` `(,x ,y z) ;(1 2 z) ``  
  
`` ` `` is a form of reader macro
`eval` takes in a list and run it as a program
```Lisp
(setf a '(1 2 3))
(atom a) ; NIL
(atom 'a) ; T
```
```Lisp
'(a b c) ; make a list with the symbol a, b and c
(list 'a b c) ; make a list with the symbol a, value of b and value of c
`(a ,b ,c) ; same as above
```
Use `#'` ts followed by the name of a function to fetch the function object

> The function names and the names of variables storing a function is not in the same namespace.
# Functions
Local variables/functions: LET, FLET
```Lisp
(let ((x 10) ; local variables
  (y 20))
(+ x y))
(flet ((func1 (params1…) (body1…)) ; local functions
		(func2 (params2…) (body2…))
	(expression using func1 and func2)))
```
Define function: `defun funcname (params) (body); return the last value evaluated in body`
Closure:
```Lisp
(defun make-add-x (x)
    (flet ((add-x (y) (+ x y)))
        #'add-x))
(setf (fdefinition 'fn) (make-add-x 3)) ; fdefinition: returns the function object associated with the given name
(princ (fn 4))
```
```Lisp
(defun make-counter (&optional (value 0))
 (flet ((counter () (setf value (+ 1 value))))
 #'counter ))
(setf (fdefinition 'c1) (make-counter))
(print (c1)) ; 1, c1 needs the parentheses, or i
(print (c1)) ; 2
(print (c1)) ; 3
```
progn: evaluate a sequence of expressions in order and return the value of the last expression
```Lisp
(progn
  (print "Hello World!")
  (setq result (some-function)))
```
# Macros
```Lisp
; definition
(defmacro setq2 (v1 v2 e)
	(list 'progn
		(list 'setq v1 e)
		(list 'setq v2 e)))
; application
(macroexpand '(setq2 x y (+ 1 3)))
(setq2 x y (+ 1 3))
```
**Reader Macro:** a feature that allows programmers to extend the syntax of the language by specifying how input text should be transformed into Lisp data structures before being processed by the compiler or interpreter
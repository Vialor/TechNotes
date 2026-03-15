> run prolog: swipl
# Concepts
**term**:
	**atomic term**:
		**constant**: **atom**(a symbolic value) or integer
		**variable**: a string starting with an uppercase letter
			**instantiation**: binding of a variable to a value
	**compound term / structure**: `functor(param tuple)`

**fact**: `food(pizza). // pizza is a food`
**rules**: `meal(X) :- food(X). // all food is meal`
	`<then structure> :- <if structure>`
**query**: `?- meal(pizza). // is pizza a meal?`
# Grammar
**is operator** vs **=**: `year is 2000`
	unification: variable = variable
	arithmetic evaluation: variable is expression
**cut operator**: `!` prevents backtracking to any choice points before the cut, effectively eliminating alternative solutions
	`a(X),!,b(Y).`
## logic operator
conjunction: `A, B.`
disjunction: `A; B.`
negation: `\+. A`
if-then: `condition -> then_goal.`
if-then-else: `condition -> then_goal ; else_goal.`
## List
```prolog
[head | tail] // head: first element, tail: rest elements

// in list
member(X, [X|_]).
member(X, [_|T]) :- member(X, T).

// merge two list
append([],X,X).
append([X|Y],Z,[X|W]) :- append(Y,Z,W).

// reverse a list
reverse([X|Y],Z,W) :- reverse(Y,[X|Z],W).
reverse([],X,X).
```
# REPL
`more file.pl`: import files
## `trace.`
prolog is using backtracking
Call, Exit, Redo, Fail
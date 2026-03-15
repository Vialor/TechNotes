> CSP = (X, D, C)  
> X: a set of variables; D: a set of domains; C: a set of constraints  
> Find an domain assignment for the variables allowable by the constraints  

Types of variables: Discrete, Continuous
Types of constraint based on the number of variables: **Unary constraints, Binary constraints, Higher-order constraint, Global constraint** (a constraint that captures a relation between a non-fixed number of variables**)**
# Approach CSP
state: a domain assignment of some variables
goal state: all variables have been assigned values and all constraints are satisfied
action: assign a value to an unassigned variable that does not conflict with current assignment
can use depth first search
## Other Tools
**Constraint Graph**: variables represented by nodes, constraints represented by arcs; each arc ties together _2 variables_.
**Constraint Hypergraph**: variables represented by nodes, constraints represented by boxes and arcs that tie together _3 or more variables_.
**Auxiliary variables & Domains for Auxiliary variables**
**Neighbors**: 2 or more variables share at least a constraint
## Complexity Analysis
number of variables: n; current depth: l; number of domain values: d
Brute force:
branching factor: (n - l) * d  
time complexity: n! * d^n  
space complexity: d * n^2  
Variable assignments are _commutative_, then one variable at a time:  
  
_commutative:_ Order of any given set of actions has no effect on the outcome.
branching factor: d  
time complexity: d**n  
space complexity: n  
## General Backtracking Solution (Commutative)
```Python
def solveCSP(CSP):
	assignment = {} # variables: values
	CSP = (variables: List[variable],
				domains: Dict[variable, List[value]],
				constraints: List[constraint])
	
	def backtrack():
		if isComplete(assignment):
			return assignment
		variable = getUnassignedVariable()
		for value in orderDomainValues(CSP.domains[variable]):
			if isConsistent(variable, value):
				assignment[variable] = value
				inferences = inference(CSP.constraints, variable)
				if inferences is not None:
					add inferences to CSP
					result = backtrack()
					if result is not None:
						return result
					remove inferences from CSP
				del assignment[variable]
		return None
	return backtrack()
```
## Heuristics to Improve Efficiency
- **getUnassignedVariable -** Variable ordering
    **Minimum Remaining Values MRV**: choose the variable with fewest legal values
    **Degree heuristic**: choose the variable that has the most unassigned neighbors
- **orderDomainValues -** Value ordering
    **Least Constraining Value**: choose domain value that rule out fewest values in the  
    domains of neighbors  
- **inference** - Tree pruning (detect early failures, reduce domain size, assign values)
    **Forward Checking**:  
    after an assignment, update the domain values of its neighbors; return failure when there is a variable has no legal value left.  
    **Constraint Propagation**: (More powerful than Forware Checking):  
    **Arc Consistency:** a type of Constraint Propagation  
    **AC-3:** most popular Arc Consistency  
    Check if all arcs consistent  
    If X loses a value, neighbors of X need to be rechecked.  
    (**arc X→Y is _consistent_ iff for every value x of X there is some allowed value y of Y that satisfies the constraints between X and Y)
    
```Python
def AC-3(arcs) -> bool: # return if inconsistency in any arc
	while arcs:
		x, y = arcs.pop()
		if revise(CSP, x, y):
			if len(CSP.domains[x]) == 0:
				return False
			for neighbor in neighbors[x] and neighbor != y:
				arcs.append(neighbor, x)
	return True
def revise(CSP, x, y): # return if x domain changes
	for vx in CSP.domains[x]:
		if no vy in CSP.domains[y] such that (vx, vy) satisfy constraint between x, y
			delete vx from CSP.domains[x]
			return True
	return False
```
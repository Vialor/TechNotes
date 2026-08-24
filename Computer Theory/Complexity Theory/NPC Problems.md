**Eulerian trail** (or **Eulerian path**) is a trail in a finite graph that visits every edge exactly once (allowing for revisiting vertices).
**Eulerian circuit** (or **Eulerian cycle**) is an Eulerian trail that starts and ends on the same vertex.
**Hamilton trail** is a trail in a finite graph that visits every node exactly once.
**Hamilton circuit** is an Hamilton trail that starts and ends on the same vertex.
**HAMPATH**: Decide if there is a Hamilton circuit in a graph.
**TSP Traveling Salesman Problem**: Find a Hamilton trail in a weighted graph under a certain cost
	$HAMPATH\leq_p TSP$
- - -
**3CNF 3 Conjunctive Normal Form**: $(A\lor B\lor C)$ chained by $\land$
**CSAT Circuit Satisfiability**: Decide if in a Boolean circuit there is a set of inputs such that all the output are true
**3SAT 3 CNF Satisfiability**: 
	$CSAT\leq_p 3SAT$
	$2SAT\in P$
- - -
**3COL 3 Colorable**: Are nodes in a graph 3-colorable?
	$2COL\in P$
- - -
In graph $G = (V, E)$, a **Vertex Cover** is a set $S\in V$ s.t. $\forall (u, v)\in E$, either u or v or both are in E 
**VC**: Decide if there is a S s.t. |S| <= k
	$3SAT\leq_p VC$
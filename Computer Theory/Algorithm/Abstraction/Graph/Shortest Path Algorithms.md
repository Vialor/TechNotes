# Dijkstra Algorithm
> Find shortest path from A to other nodes in a **non-negative-cost** directed/undirected graph.    
> 
> **uniform-cost search**: find the shortest path from A to B, similar to Dijkstra algorithm

Use min-priority queue as the frontier and adjust the cost of nodes in the frontier when expanding it. Reference to general iterative solution of uninformed search in [[Uninformed Search & Informed Search]]

O(|E|log|V|)
# Bellman-Ford Algorithm
> In a directed/undirected graph, detect any **negative cycle**, or find shortest path from A to other nodes.  
> More powerful than Dijkstra’s Algorithm as it allows negative-cost edges.  

O(|V| * |E|)
# Floyd-Warshall Algorithm
> Find shortest paths from any node to any node in a directed/undirected graph that has **no negative cycles**.  
> It is a general version of Dijkstra’s Algorithm.  

Dynamic programing:
$dp_{ij}^k$: cost of shortest path from v_i to v_j with intermediate vertices in {v_1, v_2, ..., v_k}
$$
dp_{ij}^k =
\begin{cases}
w_{ij}, & \text{if } k = 0 \\
min(dp_{ij}^{k-1}, dp_{ik}^{k-1}+dp_{kj}^{k-1}), & \text{if } k > 0
\end{cases}
$$
O(|V|^3)

## More Variations
**1. Predecessors**:
$pred_{ij}^k$: the predecessor node in the shortest path from v_i to v_j with intermediate vertices in {v_1, v_2, ..., v_k} (each vertex is used at most once)
$$
\begin{align}
pred_{ij}^0 &=
\begin{cases}
None, & \text{if } i = j \text{ or } w_{ij} = \infty \\
i, & \text{otherwise}
\end{cases} \\
pred_{ij}^k &=
\begin{cases}
pred_{kj}^{k-1}, & \text{if } dp_{ij}^{k-1} < dp_{ik}^{k-1}+dp_{kj}^{k-1} \\
pred_{ij}^{k-1}, & \text{otherwise}
\end{cases}
\end{align}
$$

**2. Negative Cycle**: If there is a negative cycle, then in the very end, there is an i such that $dp_{ii} < 0$ 

**3. Transitive Closure**: Find if there is a path from any node to any node
$$
tc_{ij}^k =
\begin{cases}
\text{if there is an immediate path from i to j}, & \text{if } k = 0 \\
tc_{ij}^{k-1}\lor (tc_{ik}^{k-1}\land tc_{kj}^{k-1}), & \text{if } k > 0
\end{cases}
$$
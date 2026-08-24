# Strongly Connected Component SCC
**Strongly Connected Component SCC**: a maximal set of vertices from a directed graph where you can go from any vertex to any other vertex
**SCC Meta-graph**: Change every SCC in the graph into a single vertex
## Observation:
1. Given SCCs C and C', if there is an edge v->u from C to C', then any node in C has a higher finishing time than every node in C'.
2. G and GT have the same SCCs. (GT is G with reversed edges)
## Algorithm:
Find_SCC(G):
1. call DFS(G) to compute finishing times u.finishing_time for all u
2. compute GT
3. call DFS(GT), but in the main loop, consider vertices in order of decreasing u.finishing_time (as computed in first DFS)
4. output the vertices in each tree of the depth-first forest formed in second DFS as a separate SCC
O(V + E)
# Minimum Spanning Tree MST
> **Minimum spanning tree** **MST:** a subset of the edges of a connected, edge-weighted undirected graph that connects all the vertices together, without any cycles and with the minimum possible total edge weight

A **Cut** C=(S_1, S_2) in a connected graph G=(V, E), partitions the vertex set V into two disjoint subsets S_1, and S_2. A Cut C can also be represented by a set of edges to be removed to form the disjoint subsets.
**The Cut Property**: The edge of minimum weight in the edge set of Cut C belongs to all MSTs of G
# MST Algorithms
> Goal: find a minimum spanning forest of an undirected edge-weighted graph
## Kruskal’s Algorithm
> good for graph with *less edges*, O(ElogE+Eα(V)) = O(ElogE)

How: each step adds *the lowest-weight edge that will not form a cycle* to the current MST, which is basically sorting edges (O(ElogE)) + union find O(Eα(V))
Reference: [[Disjoint-set Forest]]
## Prim's Algorithm
> good for graph with *more edges*, O(ElogV)

How: each step adds *the node neighboring already added nodes and with lowest-weight edge* to the current MST, which is basically expanding frontier with a priority queue
Reference: [[Uninformed Search & Informed Search]]
**Clustering**: divide the objects into clusters based on similarities
**Association**: discover the probability of the co-occurrence of items in a collection
# Clustering
## Problem Analysis
Point assignment: maintain a set of clusters and new points are assigned to the *nearest* cluster
	**centroid**: the average of a cluster of points
Hierarchical:
	Agglomerative / bottom up: initially each point is a cluster, then learn to $combine$ them
	Divisive / top down: initially there is only one cluster, then learn to $split$ them

Euclidean
Non-Euclidean

In-memory clustering
Large-data clustering
## K Means
A problem formation where given Euclidean space and k=number of clusters, find the *best* cluster centers.
	The exact solution is NP-hard
	*Best*:
		**Inertia:** sum of squared errors for each cluster
			≥ 0, lower inertia, denser the clusters 
		**Silhouette Score**: measure of how far apart clusters are
			-1~1, higher the score, more separated the clusters
### Approximate Solution: Lloyd's Algorithm / K Means Algorithm
1. Initialize k centroids in your data
2. Assign each point to the nearest centroid, which forms k clusters
3. Move each centroid to the center of its cluster.
4. Repeat steps 3-4 until your centroids converge.
# More
_主成分分析_（principal component analysis）
_因果关系_（causality）和_概率图模型_（probabilistic graphical models）
generative adversarial networks
# Techniques
[[Dimensionality Reduction]]
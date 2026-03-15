The complete model G is made up by a series of weak models $G_t$
## Adaptive Boost AdaBoost
>each $G_t(x)$ is a one-level decision tree (and hence called **decision stump**)

Given a training data: $\{(x_i,y_i)\}_{i=1}^N$, $y_i\in\{-1,1\}$
1. initialize equal weights for all observations
	$w_i=1/N$
2. for t = 1...T:
	1. Train a stump $G_t$ using weight $w_i$
	2. Compute the misclassification error adjusted by $w_i$
		$err_t={\sum_{i=1}^N w_i I(y_i\neq G_t(x_i)) \over \sum_{i=1}^N w_i}$
	3. Compute the weight of current tree
		$α_t = log({1-err_t\over err_t})$
	4. rewrite each observation based on prediction accuracy (The sequential part of boosting)
		$w_i\leftarrow w_i exp[\alpha_t I(y_i\neq G_t(x_i))]$
3. final prediction
	$G(x) = sign[\sum_{t=1}^T \alpha_t G_t(x)]$
## Gradient Boosted Decision Tree GBDT
>each $G_t(x)$ is a multilevel decision tree

#TODO
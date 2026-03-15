# Regression
>**Regression**: output variables are continuous  
	e.g. find the best-fit line to represent the overall trend in the data  
## Linear Regression
$y = w_0+w_1x_1+w_2x_2+...+w_nx_n = w^T x$﻿, w is the weight or **model coefficients**
Goal: find the model coefficients that minimizes the loss function
When the loss function is MAE or MSE, w can be solved mathematically
# Classification
>**Classification**: output variables are discrete  
	e.g. find the best possible decision boundary to separate two classes  
>**Multi-label classification**: classification when classes are not exclusive to each other
	e.g. tagging
## K Nearest Neighbors KNN
the output of sample x depends on the votes of its nearest k neighbors.
## Decision Tree
Use Case: small number of features, small sample size
Goal: build the decision tree that minimizes the loss function
1. Ideal approach: Considering every possible partition of the feature space (computationally infeasible).
2. greedy algorithm approach: Recursive binary splitting.
    1. Begin at the top of the tree.
    2. For _every feature_, examine _every possible cutpoint_, and choose the feature and cutpoint so that the resulting tree has the lowest possible mean squared error (MSE). Make that split.
    3. Examine the two resulting regions. Once again, make a **single split** (in one of the regions) to minimize the loss function.
    4. Keep repeating Step 3 until a _stopping criterion_ is met:
        - Maximum tree depth (maximum number of splits required to arrive at a leaf).
        - Minimum number of observations in a leaf.
        - The leaf is pure: $Variance(y)\leq \epsilon$.
3. an even naiver implementation
```Python
def learnDecisionTree(examples, attributes, parentExamples):
	if len(examples) == 0:
		return pluralityValue(parentExamples)
	elif all examples have the same classification:
		return the classification
	elif len(attributes) == 0:
		return pluralityValue(examples)
	else:
		A = a in attributes such that maximizes purity(a, examples)
		tree = treeNode(A)
		for v in A.domain:
			exs = [e for e in examples and e.A = v]
			subtree = learnDecisionTree(exs, attributes-A, examples)
			add subtree to tree labeling A=v
		return tree
```
pluralityValue:
	selects the majority output value among a set of examples, breaking tie randomly
**purity 纯度**: **Entropy** or **Gini impurity** from [[Evaluation]]
	观察通过特征A来分割数据集后，目标变量V的不确定性的变化
	ID3: **Information gain**
		$gain(V) = H(V) - \Sigma_{a\in A} P(A=a)H(V|A=a)$
	CART: **Gini index**
		$gini(V, a) = \Sigma_{a\in A}P(A=a)Gini(V|A=a)$
## Logistic Regression
Assumption: binary classification, decision boundaries are linear
基本思想：构建一个线性分类器，并利用sigmoid函数将线性输出转换为0~1之间的概率值，以确定样本的分类。
### Training
#### Situation 1
find weight that maximizes P(weight | data)
>weight comes from uniform distribution
>The data (features) are independent and identically distributed (i.i.d.), and the response variable (target) follows a Bernoulli distribution

**Maximum likelihood estimation MLE**
logistic loss: $log(1+e^{-yW^Tx})$
#### Situation 2
>prior knowledge on weight

**Maximum a posteriori estimation MAP**
## Support Vector Machine SVM
binary classifier that can work for non-linearly separable data 
hinge loss

Hyperplane: $w^Tx+b = 0$, b is the distance from the origin, w is the normal vector of the plane
Distance from any point to the hyperplane: $r = {|w^Tx+b|\over ||w||}$
Distance from sample x_i to the hyperplane: $r_{w,b} = min_i{y_i(w^Tx+b)\over ||w||}$, note that $y_i\in \{-1, 1\}$

Set the margin to 1

Want to maximize the margin: $max_{w,b}r_{w,b}$
Margin of a hyperplane: the distance the hyperplane and one side of the nearest the data to it $\gamma = {1\over ||w||}$
$max_{w,b}{2\over ||w||}$

hard margin SVM
soft margin SVM
slack variable
![[Pasted image 20240924184935.png]]
# Ensemble
What: a group of base learners that collectively achieve a better final prediction
Why: Motivation: anti-noise, avoid overfitting
How:
	Ensembled in parallel: [[Bagging (Bootstrap Aggregation)]]
	Ensembled in sequence: [[Boosting]]
# Recommending System


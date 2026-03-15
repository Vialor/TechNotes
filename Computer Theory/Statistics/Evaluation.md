# Accuracy Measures: Confusion Matrix

| Predicted\Actual | Positive           | Negative           |
| ---------------- | ------------------ | ------------------ |
| Positive         | True Positives TP  | False Positives FP |
| Negative         | False Negatives FN | True Negatives TN  |
**Accuracy 精度** = (TP+TN)/(TP+FP+FN+TN)
**Error Rate 错误率** = (FP+FN)/(TP+FP+FN+TN)
- - -
**Precision 查准率** = TP/(TP+FP) finding TPs among all positive predictions
	e.g. Censoring: we don't want to wrong the good guy 
**Recall / TPR True Positive Rate 查全率** = TP/(TP+FN)
	e.g. Security: we don't want to miss a potential threat
**FPR** **False Positive Rate** = FP/(FP+TN)
**F1 Score** = (2 * precision * recall) / (precision + recall)
	a harmonic mean of precision and recall, between 0 and 1

**Base Rate Fallacy**: evaluation ignoring the impact of prior probability (the distribution of actual positive and negative)
- - -
**ROC Curve** **(Receiver Operating Characteristic Curve)**: An ROC curve plots TPR vs. FPR at different classification thresholds.
**AUC (Area Under the ROC Curve)**: AUC is the integral of ROC curve
# Dispersion Measures
**Variance**: $\sigma^2=E((X-E(X))^2)=E(X^2)-E(X)^2$﻿  
$\sigma^2={\Sigma (x-\mu)^2 \over n}$﻿  
**Standard deviation SD**: $\sigma$﻿  
(n: sample size, x: each value from the population, µ: expected value of of x)  
**Standard error of the mean SEM**: how far the sample mean of the data is likely to be from the true population mean. The SEM is always smaller than the SD.  
$SEM = {\sigma\over \sqrt{n}}$
# Error Measures: Loss Functions
## Regression Error
MSE Mean Squared Error = ${1\over n}\Sigma_{i=1}^N(\hat{y_i}-y_i)^2$﻿
MAE Mean Absolute Error = ${1\over n}\Sigma_{i=1}^N|\hat{y_i}-y_i|$﻿
Huber loss:
$$
L_\delta(y, \hat{y}) =
\begin{cases}
\frac{1}{2}(y - \hat{y})^2 & \text{if } |y - \hat{y}| \leq \delta \\
\delta \cdot |y - \hat{y}| - \frac{1}{2}\delta^2 & \text{if } |y - \hat{y}| > \delta \end{cases}
$$
## Classification Error
### Purity Measures
>v represents all the possible value of V
>P(v) is a short form for P(V=v)

**Gini Impurity**: $Gini(P, V)=1-\Sigma_vP(v)^2$
	a measure of the probability of random 2 samples in different classes
	$Gini \in [0,1)$; 0: V is pure

**Self Information**: $I(P, v)=−lg(P(v))$
	a measure of how surprising for a event to happen
	$I(v) \in [0, \infty)$; 0: v is not surprising at all or containing very little information
**Entropy**: $H(P, V) = -\Sigma_vP(v)I(P, v)$﻿
	a measure of the uncertainty of a random variable V
	$entropy \in [0, lg(n)]$, n is the number of potential value of V; 0: V is pure or very little information uncaptured by the model
### Classification Error
**Hinge Loss**: $L=max(0,1 − y⋅\hat y)$, $y\in {-1, 1}$ is the actual label, $\hat y$ is the predicted value
	Used in binary classification
	$y⋅\hat y >= 1$ Prediction is correct and away from the boundary
	$y⋅\hat y < 1$ Prediction is wrong or close to the boundary

**Cross Entropy Loss**: $E(P, Q, V)=\sum_{i=1}^nQ(v_i)I(P, v_i)$,  n is the number of potential value of V, Q is the actual distribution, P is the predicted distribution
	A measure of the closeness of distribution P and Q
	Intuition: Weighted(based on actual distribution) average of how surprising we find for each value of variable V
	Cross entropy loss over m samples: ${1\over m}\sum_{k=1}^m E(V_k)$
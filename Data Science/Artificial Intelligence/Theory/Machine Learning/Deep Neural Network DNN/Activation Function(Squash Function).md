>**Motivation**: to explore beyond the linear relationship between inputs and outputs
>**Requirements**: domain is R, continuously differentiable, monotonically increasing

**Saturation function**: derivatives close to 0 as input goes to inf or -inf
	could lead to **vanishing gradient problem** 
Sigmoid: $\sigma(x) = {1\over (1+e^{-x})}$
Tanh: $e^{2x}-1\over e^{2x}+1$

**Non-zero mean function**: output does not have a mean value of zero across its input range
	could be bad for convergence because it has a consistent bias on certain inputs of activation functions (ReLU has a consistent non-zero bia)
	could lead to **exploding gradient problem**
Softplus: $softplus(x)=ln(1+e^x)$﻿
ReLU Rectified Linear Unit: $RelU(x)=max(0,x)$﻿  
	often used nowadays, though not differentiable at 0
P-ReLU Parametric ReLU: $P-ReLU(x) =\begin{cases}x, & \text{if } x > 0 \\\alpha x, & \text{if } x \neq 0\end{cases}$
	α is a hyper parameter
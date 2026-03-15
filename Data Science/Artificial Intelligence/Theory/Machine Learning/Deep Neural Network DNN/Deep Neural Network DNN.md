[[Weight Initialization]]
# Neuron
> the basic unit of neural networks

Input Links: activations of previous layers + weighted biases
Input Function: in_i
Activation Function: g
	[[Activation Function(Squash Function)]]
Output: a_i
Output Links
$g(in_i(Input Links))\rightarrow output$﻿
  
==Example of a linear in_i:==
$g(\sum_jw_{ji}a_j-bias)\rightarrow a_i$﻿  
Weighted sum tells us the _pattern_ to expect from previous layer  
Bias tells us how significant the weighted sum should be before the neuron starts getting meaningfully active
## Types of Neuron
- **Perceptron**:
	- activation function is a step function that produces a binary output
	- update weights only when the prediction is wrong. The update is based on the binary error
- **Adaline**:
	- use a continuous activation function
	- update weights based on the **mean squared error** between the continuous predicted output and the actual target during training
# Epoch
> The neural network will be trained for many epochs until loss function yields a value below the threshold.

==Example of what happens in an epoch in an **MLP Multi-Layer Perceptron** (Feed-forward Network):==
There are three layer to consider in each iteration: j, i, k, in that order
1. Apply **loss function** on each output neuron:
	$a_i=h_i(W, X)$ Predicted output﻿
	$Err_i=y_i-a_i$ Error = actual output - predicted output
	$L_i={1\over 2}Err_i^2$ The **Loss function** - Squared Error (1/2 is only for math convenience)
2. Take partial derivative of loss function with respect to the weight of each link between the hidden layer and the output layer
	${\partial L_i\over\partial W_{ji}}={\partial L_i\over\partial a_i}{\partial a_i\over\partial in_i}\times{\partial in_i\over\partial W_{ji}}=-\Delta_i\times a_j$ Where $\Delta_i = g'(in_i)Err_i$
	Intuition: find the direction to minimize loss
3. **Back propagation of error**: for non-output layers, Err is not available, and must be propagated from the next layer (k)
	${\partial L_i\over\partial W_{ji}}=-\Delta_i\times a_j$ Where $\Delta_i=g'(in_i)\sum_kW_{ik}\Delta_k$
4. **Gradient descent**: adjust weights in the direction of the steepest decent so that the loss function value is reduced
	$v_{ji}\leftarrow \beta v_{ji}-\alpha(0.0005W_{ji}+{\partial L_i\over\partial W_{ji}})$
		$\beta\in[0,1]$ is the **momentum**, which mitigate the effect of local "lowland" on gradient descent, it is usually set to 0.9
		0.0005 is the **weight decay** or **L2 Regularization**
		$\alpha\in[0, 1]$ is the **learning rate**
	$W_{ji}\leftarrow W_{ji}+v_{ji}$
	**Gradient descent:**
		**Batch Gradient Descent**: go through all training samples to compute average delta 
		**Stochastic gradient descent**: randomly choose one training sample to compute delta
		**Mini-batch gradient descent**: randomly divide all training samples into mini batches of size k, iterate over them and compute average delta

| 特性             | **Batch Gradient Descent** | **Stochastic Gradient Descent(SGD)** | **Mini-batch Gradient Descent** |
| -------------- | -------------------------- | ------------------------------------ | ------------------------------- |
| **每epoch的样本量** | 全部训练样本                     | 单个样本                                 | 全部训练样本                          |
| **更新频率**       | 每次遍历完整数据集后更新一次             | 每处理一个样本后立即更新                         | 每处理一个 mini-batch 后更新一次          |
| **内存需求**       | 高，处理整个数据集                  | 低，每次处理一个样本                           | 适中，每次处理一个 mini-batch            |
| **收敛速度**       | 慢，梯度估计精确，但更新频率低            | 快，但不稳定，容易振荡                          | 适中，较平滑，能平衡收敛速度和稳定性              |
| **适用场景**       | 小数据集                       | 大规模数据集，要求实时更新                        | 中等规模的数据集，平衡稳定性和效率               |
Stopping criterion: loss (or error) is below some threshold, fixed # of epochs, loss (or error) does not change much between successive epochs, etc.
# Neural Network Structure
**Feed-forward Neural Networks FNN**
	A directed acyclic graph with no loops.
	Output from the units in one layer are fed to the inputs of the units in successive layers in one direction.
[[Convolutional Neural Networks CNN]]: images, videos

**Recurrent Neural Networks RNN**
	A directed graph with loops.
	Feed intermediate or final outputs back to the input of neurons in previous layers.
	Have internal states or memory.
[[Recurrent Neural Network RNN]]: Natural Language Processing NLP, time series forecasting

Long short-term memory (LSTM)
[[Generative Adversarial Networks GAN]]
[[Graph Neural Network GNN]]
[[Transformer]]



# Features of image inputs
1. Locality of pixel dependencies: Adjacency of pixels is important
2. Input layer is extremely large
3. (To certain degree) **Spatial invariance**: shifting the input signal results in an equally shifted output signal  
    There is also  **Temporal invariance** for audio inputs
    stationarity of statistics
# CNN Solution
**kernel/filter**: a pattern of weights that is replicated across multiple local regions
**convolution**: the process of applying the kernel to the pixels of images
**stride**: the distance between the centers of the kernels applied

**Fully Connected Layers** connect every neuron in one layer to every neuron in the previous layer, takes very long time to train
  
**Convolutional Layers** summarize a set of adjacent units from the preceding  
layer with a single value using filters  
	**Sparse connections**: Each neuron in a convolutional layer is connected to a small number of inputs from the previous layer.
	**Parameter sharing**: Weight values for the connections between a convolutional layer and the previous layer are shared among different locations for the filter

**Pooling Layers** simply performs down sampling; no activation function is associated with the pooling layer.
	**Max-pooling/Average-pooling**: output the max/average in the local region
## Convolution types
- **Valid 卷积**（有效卷积）：这是最常用的卷积模式，不进行任何填充（padding），结果矩阵的尺寸比输入矩阵小，因为卷积核只能在输入矩阵内部滑动。
- **Same 卷积**（等尺寸卷积）：通过在输入矩阵边缘填充，使得输出矩阵的尺寸与输入矩阵相同。这种模式通常用于卷积神经网络中，当需要保持输出的维度时使用。
- **Full 卷积**（全卷积）：这是在**反向传播**中常见的一种模式。在全卷积中，卷积核可以完全与输入矩阵重叠，即在输入矩阵的边界之外继续滑动。这种操作相当于将输入矩阵周围加上了额外的填充，使得输出矩阵的尺寸比输入矩阵大。
## Filters
Sharpen: $\begin{bmatrix} -1 & -1 & -1 \\ -1 & 8 & -1 \\ -1 & -1 & -1 \end{bmatrix}$
Identity: $\begin{bmatrix} 0 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 0 \end{bmatrix}$
Blur: ${1\over 8}\begin{bmatrix} 1 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & 1 \end{bmatrix}$
Edge Detection
# Implementation
## Math background
**Cross-Correlation**: $A\star B$, apply filter B on A
**Convolution**: $A\ast B = A\star rot180(B)$
>In CNN, when we say convolution, we actually use cross-correlation 
## Convolution Layer
Y: output matrix
X: input matrix
K: filter matrix
### Forward propagation
i for output channel, j for input channels
$Y_i = Bias_i + \sum_{j}X_j \star K_{ij}$
### Backward  propagation
$$
\begin{align}
{\partial E\over\partial K_{ij}} = X_j \star {\partial E\over \partial Y_i} \\
{\partial E\over\partial Bias_{i}} = {\partial E\over \partial Y_i} \\
{\partial E\over\partial X_{j}} = \sum_i({\partial E\over \partial Y_i} \star_{full} K_{ij})
\end{align}
$$
## Pooling Layer
Backward propagation
# [[Modern CNN]]
**Lasso / L1 Regularization**: weight comes from Laplace distribution
$Loss\leftarrow Loss+\lambda\Sigma|w|$
reduce **overfitting** by reducing some coefficient to zero and creating a sparse model

**Ridge / L2 Regularization**: weight comes from Normal distribution
$Loss\leftarrow Loss+\lambda\Sigma|w|^2$
reduce **overfitting** by penalizing higher coefficients

**Elastic Net Regularization**: L1 + L2
$Loss\leftarrow Loss+\lambda_1\Sigma|w|+\lambda_2\Sigma|w|^2$

**Dropout**: 在训练过程中向前传递时随机丢弃一部分神经元，避免神经网络对特定神经元的依赖，从而增强网络的泛化能力
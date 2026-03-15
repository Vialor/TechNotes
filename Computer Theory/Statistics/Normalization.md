>Mapping discrete values x_i to \[0, 1]

Softmax: $x_i \leftarrow {e^{x_i}\over\Sigma_j e^{x_j}}$
	LogSoftmax(\[0,1]->\[-inf,0]): $x_i \leftarrow x_i-log(\Sigma_j e^{x_j})$
		log of softmax, which punishes large numbers to help numerical stability
Minmax: $x_i\leftarrow {x_i-x_{min}\over x_{max}-x_{min}}$
Standard: $x_i\leftarrow {x_i-\mu\over\sigma}$, as in the normal distribution
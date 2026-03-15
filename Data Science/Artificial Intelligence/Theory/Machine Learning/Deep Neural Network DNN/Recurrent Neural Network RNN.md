# Sequence Models
>一个数学观察：	$P(x_1,...,x_T)=\Pi^T_{t=1}P(x_t|x_{t-1},...,x_1)$
>一个前提：Stationary Dynamics 序列的动力学是静态的，换句话说每个时间点$x_t$的值可以由其过去的观测值（即$x_1,...,x_{t-1}$​）所决定。

**Markov Condition**: $x_t$ can be predicted based on $x_{t-1},...,x_{t-τ}$
==**Autoregressive models 自回归模型**==
	$P(x_1,...,x_T)=\Pi^T_{t=τ}P(x_t|x_{t-1},...,x_{t-τ})$
**First Order Markov Model**: when τ = 1, $P(x_1,...,x_T)=\Pi^T_{t=1}P(x_t|x_{t-1})$

Consider all past history:
==**Latent autoregressive models 隐变量自回归模型**==
	$P(x_1,...,x_T)=\Pi^T_{t=1}P(x_t|h_{t}),\space h_t=g(h_{t-1},x_{t-1})$
# Text to Sequence Data
**词元 Token**: Basic unit of text
**语料 Corpus**: 一个包含大量文本的数据集，可能包括重复的词元。
**词表 Vocabulary**: 一个有限大小的词元集合，通常用于 NLP 任务，并且映射到索引以供模型使用。


vector-sequence model
sequence-sequence model
sequence-vector model
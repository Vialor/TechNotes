# 业界主流大模型和关键技术
## GPT generative pre-trained transformer
By OpenAI
only decoders

| Models    | Year      | Parameter size | Features                                      |     |
| --------- | --------- | -------------- | --------------------------------------------- | --- |
| GPT-1     | 2018      | 1.17亿          | 无监督预训练+监督微调                                   |     |
| GPT-2     | 2019      | 15.42亿         | 主推zero-shot能力，训练数据集更大                         |     |
| GPT-3/3.5 | 2020/2022 | 1750亿          | 96层transformer-decoder                        |     |
| GPT4      | 2023      | 推测3000亿~1万亿    | 多模态，predictable scaling，长上下文理解，牺牲速度大幅提升逻辑推导能力 |     |
**predictable scaling VS emergent ability**
	**emergent ability 涌现能力**：小模型上表现为瞎猜，但模型参数足够大后，模型能力显著提升的现象
	**predictable scaling**：当模型参数、数据量、算力按某种比例增加时，性能提升大致平滑
		用更小的模型预测大模型的能力，以避免模型粒度的繁琐微调和巨大的试错开销
## LLaMA
By Meta
### 层归一化：RMSNorm ->训练稳定性
LN Layer Normalization：Transformer模型中，每个子层后都会接一个残差模块，并有一个LN在每一个单词的embedding范围内做归一化
RMSNorm Root Mean Square Layer Normalization: 去掉了LN公式的减均值部分，加速模型收敛提高效率

LN位置：
	Pre-Norm: LN在残差连接residual之前
	Post-Norm：LN在残差连接residual之后
### 激活函数：SwiGLU -> 提高模型性能
**Gated Linear Units GLU 门控线性单元**引入了两个不同的线性层，其中一个经过sigmoid后与另一个线性层输出逐元素相乘。
	$GLU(x,W,V,b,c) = \sigma(xW+b)\otimes(xV+c)$：$\sigma(xW+b)$门控$(xV+c)$的输出
**Swish激活函数**：$Swish_\beta(x)=x\sigma(\beta x)$，$\beta$是一个可学习参数
**SwiGLU**：将GLU的激活函数改成Swish
	$SwiGLU(x,W,V)=Swish_\beta(xW)\otimes(xV)$
### 位置编码：RoPE Rotary Position Embedding -> 建模长序列数据
PE: 位置编码结果直接加到 token embedding 上，绝对位置编码
RoPE: 在注意力的 Query/Key 上做二维旋转变换来引入位置信息，相对位置编码
### 注意力机制：GQA 分组查询注意力 -> 平衡性能效率
MHA Multi Head Attention, MQA Multi Query Attention, GQA Grouped Query Attention
## Mistral
By Mistral AI

Mistral 7B, Mistral 7B-Instruct
Mistral 8×7B, Mistral 8×7B-Instruct
### MOE Mixture of Experts
用稀疏的Switch FFN层替换了Transformer中密集的FFN层，并增加了门控网络。

GateNet门控网络：根据输入数据特征，动态决定哪个专家模型应该被激活；门控网络选出topk个专家网络，加权求和输出
Experts专家网络：一组独立模型，每个模型都负责处理某个特定子任务

MOE模块：ST Switch Transformer，EC Expert Choice（专家选择top-k tokens，而不是tokens选择top-k专家）
## Phi系列小模型
By Mictosoft

教科书级别的数据集质量
## 中文模型
阿里巴巴：通义千问
智谱AI：GLM-4
Deepseek
Chinese LLaMA & Alpaca
# LLM微调和对齐原理
## 微调算法
**全参数微调**：全部参数进行调整
**参数高效微调**：部分参数进行调整
	**Addition**：引入额外的可训练神经模块或参数
		Prefix-tuing, Prompt tuning, P-tuning，P-tuning v2
	**重参数Reparameteriization**：将现有参数重新参数化为参数有效的形式
		**LoRA Low Rank Adaptation**：模型的矩阵参数具有低秩性，因此向transformer架构注入可训练的低秩分解矩阵，通过训练低秩分解矩阵以适配不同的任务
			$h=W_0x+\Delta Wx=W_ox+BAx$
		**Qlora Quantized LoRA**：LoRA的基础上做量化以节省内存
			NF4量化，双量化，分页优化器
	**Specification**：指定原始模型或过程中的某些参数为可训练的，冻结其他
		**BitFit**：只更新偏置项Bias参数
## 对齐算法
### RLHF
三部分：待对齐的语言模型，基于人类偏好训练的奖励模型，用于对齐人类偏好的强化学习算法
三阶段：人类反馈数据收集，奖励模型训练，强化学习微调
优化目标：智能体在交互过程中学习，目标是找到一个策略，这个策略根据当前观测到的状态来选择最佳的动作，使得动作轨迹 τ = {a₁, a₂, ...} 能获得的奖励的总和 R(τ) = ∑ₜ₌₁ᴿ rₜ 最大化。
	假设参数为 θ 的策略模型做出的轨迹 τ 的概率为 P_θ(τ)，那么目标函数可以写为：$\mathcal{J}(\theta) = argmax_{\theta} \mathbb{E}_{\tau \sim P_\theta}[R(\tau)] = \arg\max_{\theta} \sum_{\tau} R(\tau)P_\theta(\tau)$
on-policy 同策略：想要训练的智能体和与环境交互的智能体是同一个
off-policy和异策略：想要训练的智能体和与环境交互的智能体不是同一个
### 强化学习微调
**Actor Model**：待对齐模型，即策略模型，微调过程中不固定参数
**Critic Model**：评论家模型，预估中间状态的价值，一般使用上一个环节奖励模型参数为起点，微调过程中不固定参数
**Reward Model**：奖励模型，给出最终状态的奖励，微调过程中固定参数
**Reference Model**：参考模型，约束Actor，防止训偏，一般用于对齐初始化，微调过程中固定参数
交互过程：Actor根据当前轨迹生成下一个token，Critic预估新状态价值。循环此过程直到终止token，此时Reward Model给出最终状态奖励R
### PPO 近端策略优化
限制在每个训练阶段对策略所做的更改来提高训练的稳定，从而有助于收敛
通过重要性采样让采样的数据可以重复使用
### DPO Direct Preference Optimization 直接偏好优化算法
一个基础参考模型用于推理，一个policy模型（SFT后的模型本身）用于训练
loss函数中包含隐式奖励模型RM，大模型训练过程中经过loss函数反向传播去更新policy模型参数
优化目标：增加正偏好数据生成改率，降低反偏好数据生成概率

将强化学习的最大化奖励和转化成损失函数，直接在偏好数据上进行监督学习训练
### 其他算法
KTO 克服数据瓶颈
SteerLM 用户能根据指定的属性动态地控制模型输出
# RAG和向量检索算法
**RAG Retrieval-Augmented Generation 检索增强生成**

**高级RAG**：增加了检索预处理pre-retrieval和检索后处理post-retrieval，以提升RAG召回率和模型生成质量
**模块化RAG**：朴素RAG和高级RAG流程是固定的，模块化RAG将RAG功能抽象成多个独立功能模块，用户可以自由组合拼接RAG
## 主流RAG算法
**RAG Embedding**：用verctor embedding表达多模态数据
**Index技术**：在embedding数据库上构建高性能向量索引，用于加速向量查找
	HNSW图索引
	IVFPQ索引（倒排索引Inverted FIle，乘积量化Product Quantization）
**RAG Reranker**
	**Pointwise Reranker 逐点重排**：对候选文档逐个进行相关性打分
	**Listwise Reranker 列表重排**：将查询和所有候选文档同时作为输入，并输出重排寻列表

GraphRAG(by Microsoft)：知识图谱的构建和分层社区摘要生成
## 向量检索算法 ANN Approximate Nearest Neighbor
空间划分：将空间划分成多个区域，检索时只选择部分区域检索。加速检索明显，但精度损失严重，受维度灾难影响严重。
	Annoy：基于树状结构空间划分
	LSH：基于超平面空间划分
	倒排算法：基于kmeans聚类方式空间划分
量化压缩：加速检索，降低索引内存开销，但检索精度存在确定的上限瓶颈。
	标量量化 SQ：量化原始向量的每一个维度
	乘积量化 PQ：把向量分成多段子向量（子空间分解），在各子空间聚类算法建立一个码本，每段子向量用“最近的中心编号”替代
图索引：”邻居的邻居也可能是邻居“，效率和精度可以保证，但内存使用和构建成本较高
	HNSW，NSG，SSG，混存算法 DiskANN，基于GPU的CAGRA算法（Nvidia），Vamana
		DiskANN：在SSD上存每个点的原始向量和邻居关系，在DRAM上存每个点的PQ编码、高热点的临边和原始向量、元信息
	NNDescent算法近似实现了KNN图，NSG、SSG、CAGRA都用到了此算法
组合算法：SPTAG，NGT
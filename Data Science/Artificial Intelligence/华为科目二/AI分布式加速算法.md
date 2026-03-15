
# 概述
**AI分布式训练流程**：
	指定设备：初始化NPU集群
	准备数据：分布式数据采样器，确保数据不重用
	准备模型：DDP包裹模型（Distributed Data Parallel），使分布式对用户透明
	训练

**集合通信**：一组节点间的通信
	集合通信库：MPI，GLOO，NCCL，HCCL
**集合通信原语**：集合通信中的操作 `torch.distributed
	点对点 Send, Receive
	一对多：Broadcast, Scatter
	多对一：Gather, Reduce
	多对多：
		collective aggregation：AllGather（每张卡拿到所有卡的全数据）, AllReduce（每张卡拿到所有卡的数据聚合）
		permutation：AlltoAll（每卡发给所有卡不同部分的数据）

**组网结构**
	**中心化系统架构**：任意两节点间通信距离为1
		Parameter Server: 有中心节点，与其他机器通信
		Ring AllReduce: 无中心节点，单向环图，每个节点只需要与相邻节点直接通信
	**去中心化系统架构**：难以有效实现AllReduce
		算法层面解决：如AllReduce -> Decentralized-SGD

**分布式训练框架及工具**
Nvdia: NeMo-Megatron：pytorch+GPU生态（代码仓，推荐作为全参预训练）
Microsoft: DeepSpeed（独立API，推荐用于SFT，RLHF，DPO）
昇腾亲和的Megatron：ModelLink: Megatron + PTA（torch npu适配层）+ MindSpeed加速层 + ModelLink功能层

**推理框架**
TGI，vLLM

量化训练性能：
	TPS: Tokens Per Sec; TGS: Tokens/GPU/Sec
	MFU Model FLOPS Utilization：实际每秒浮点运算TFLOPS除以卡出厂峰值TFLOPS
# 训练加速算法
## 分布式并行加速算法
**异步训练**：算的快的XPU计算完梯度后单独更新模型
	花费时间少，但精度可能低于同步（延时问题：更新模型时用的是老旧梯度）
	处理延时问题：
		算法层策略，如delay compensation
		半异步训练：异步训练中适当同步以减小延时影响，如第n快的NPU更新k步后，所有模型同步一次
**数据并行 data parallel DP**：每个XPU拷贝一份模型，计算不同数据
**模型并行 model parallel**：多个XPU共同维持一个模型
	**横向~/张量~ tensor parallel TP**：同一个layer放入不同XPU
		结合sequence parallel（序列长度切分）
	**纵向~**：以layer为单位，将神经网络分到不同XPU
		**流水线~ pipeline parallelism PP**：解决纵向~同时只能由一块NPU在计算的资源浪费，XPU跑完了当前数据，继续跑下一块数据
			实现：Gpipe, PipeDream
		**虚拟流水线~ Virtual PP VPP**：将模型层切得更细，增加流水线阶段，用更多通信量来减少气泡时间
**优化器并行**：多个XPU共同维持一套optimizer states
**混合并行 Hybrid Parallel**：数据并行 + 模型并行
	自动并行 Auto Parallel：自动化混合并行
	多维自动并行 Galvatron-BMW(北大)
DeepSpeed Ulysses 长序列并行：切分输入序列，每张卡计算一部分序列
### **通信优化**
靠后部分层计算和通信重叠
LocalSGD：所有模型单独更新k iterations后，再同步
梯度压缩：每个 worker 算出本地梯度，之后压缩梯度信息（random-k, top-k, low-rank等方法），以减少通信传输的bits
### **分布式优化器**
梯度压缩
Error feedback: 梯度压缩前，将累计的本地压缩误差加到梯度上去，以应对梯度压缩的训练精度降低
## **内存优化加速算法**
### 基础概念
内存分类（pytorch）：
	静态内存：从训练开始申请到结束都不释放：参数，梯度，优化器状态等
	动态内存：训练过程中不断申请释放：激活值，算子workspace
	内存碎片：当前内存block中剩余的因无法满足新申请需求而已分配但未被使用的小内存
### 静态内存优化
**Google：Lion**：去除二阶动量
#### DeepSpeed：Zero
ZeRO = Zero Redundancy Optimizer:

| 阶段         | 优化内容    | 节省显存对象           | 主要思想                              |
| ---------- | ------- | ---------------- | --------------------------------- |
| **ZeRO-1** | 优化器状态分片 | optimizer states | 例如 Adam 的 `m`、`v` 向量在不同 GPU 上分布存储 |
| **ZeRO-2** | 再分片梯度   | gradients        | 各 GPU 仅保存自己需要更新的梯度分片              |
| **ZeRO-3** | 连参数也分片  | parameters       | 模型参数本身也分片，不再在每张 GPU 上完整保存         |
### 动态内存优化
**Megatron：重计算 rematerialization/recomputing**：丢弃部分activation以节约内存，反向传播的时候重新计算这些activation（牺牲算力）
### 其他
**CPU offloading/paging**：将部分activation暂存到CPU（牺牲NPU-CPU通信成本）
**DeepSpeed：ZeRO-Offload** 是 ZeRO-2 的扩展：把优化器状态（或部分梯度）**卸载到 CPU 内存**（主机内存）中
	**ZeRO-Infinity** 是 ZeRO-Offload 的进一步扩展：不仅可以把数据放到 CPU 内存，还能卸载到 **NVMe SSD**（甚至远程存储）
**蚂蚁：Gmlake虚拟显存管理**：提出了Virtual Memory Stitich，使用虚拟层来解决内存碎片问题
## **计算效率提升算法**
稀疏化：深度扩增（层级稀疏激活），广度扩增（专家稀疏路由），梯度冻结（停止部分参数反传）
	*扩增*：训练前半部分训练小模型，之后扩增，最后训练完整模型
低秩分解
低精度训练：fp16、int8
	处理underflow：loss scaling：把loss放大AMP倍，参数更新前再除以缩放因子（AMP Automatic Mixed Precision by NVIDIA自动判断应该放大多少倍来避免underflow）
混合精度训练：使用低精度来进行前反向的计算以获得更高的性能，使用高精度来进行精度敏感的优化器更新以保证模型训练的精度
融合算子：将连续计算融合，以减少小算子的下发和频繁的低带宽数据搬运（主要在attention部分，如FlashAttention）
# 推理加速算法
大模型推理阶段分析：Prefill+Decoding
	Prefill：计算密集型 矩阵乘矩阵
		根据输入生成第一个token，一次前向即可完成
	Decoding：访存密集型 向量乘矩阵
		生成一次token以后，不断自回归生成一个新token，直到stop token
## 量化压缩：压缩权重减少内存开销
权重激活全量化（精度低，收益高）：如W8A8（权重和激活都用8bit量化），减少了计算量：
	SmoothQuant：离群值主要分布在激活值上，权重分布相对平缓，因此把激活中的“尖峰”通过一个等价的缩放变换转移到权重里，让激活更好量化。
		核心是引入一个对角缩放矩阵 S（每个通道一个缩放因子）：$Y=XW+b=(XS)(S^{−1}W)+b$
	Outlier Suppression+
仅权重量化（精度高，收益低）：只量化模型权重，矩阵乘保持浮点计算、不减少计算，只减少显存占用和数搬运
	AWQ：权重的重要性并不相同，通过观察激活值分布而非权重来寻找最佳的每通道缩放因子，以保护1%的显著权重，从而减少量化误差
## 并行解码：计算换访存
**并行解码**：增加每轮迭代计算生成候选token数量，并通过mask attention完成多个候选token的并行校验；最终提升每轮迭代最终生成token数量，从而减少迭代次数。
	候选词生成阶段：生成几段候选词用于校验
	模型并行校验阶段：只用原模型的encoder计算单元以及相应的attention mask矩阵，验证候选词正确性

大小模型投机：Big Little Decoder：小模型多轮推理生成多个候选词，大模型做一次的并行的解码和校验  
多头注意力：Medusa：给模型额外的Medusa Heads，实现多个位置token的预测，再用原模型校验多个位置token
自投机：Lookahead：推理过程中增加额外的联想词，并生成可用的n-gram候选词组进行预测和校验，实现推理加速
## 长序列优化：优化KV减少内存开销
KV Cache 量化：KV Cache内存规整化，减少多batch时句子长短不一导致的内存碎片化
KV序列压缩：基于Attention语义相似性和稀疏性，压缩序列长度
	Streaming LLM
H2O：初始化KV缓存列表，前k个条目设为重命中，后k个条目设为最近使用的令牌。在每次解码迭代中，计算每个令牌的注意力分数，并根据这些分数更新H2列表。
Ring Attention

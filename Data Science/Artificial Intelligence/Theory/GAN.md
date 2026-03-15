Generative Models 生成模型
	Explicit density
		Tractable density
		Approximate density
			Variational
			Markov Chain
	Implicit density
		Markov Chain: GSN
		Direct: GAN 

**GAN Generative Adversarial Networks 对抗生成网络** 
	**Generator 生成器**：以随机噪声为输入，生成一个样本来迷惑判别器，使它不能区分真实样本和生成样本
		G is a network that defines a probability distribution $P_G$. The goal is to find $G^*=argmin_GDiv(P_G,P_{data})$.
	 **Discriminator 判别器**：输入真实数据和输入数据，输出一个0-1的值来判别输入数据是真实的还是生成的
		 The goal is to find $D^*=argmax_DV(G,D)$, where $V(G,D)=E_{x\sim P_{data}}[logD(x)]+E_{x\sim P_G}[log(1-D(x))]$ (expect the real distribution to be large, the generated distribution to be small)
		 Solve and find: $D^*(x)={P_{data}(x)\over {P_{data}(x)+P_G(x)}}$
		 
 In each training iteration:
 1. **Fix generator G and update discriminator D.** 
	 D learns to assign high scores to real objects and low scores to generated objects.
 2. **Fix D and update G.** 
	 G learns to fool D.
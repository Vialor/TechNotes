Training Objectives:
	MLM Masked language modeling
	NSP Next sentence prediction

Positional Embedding
Token Embedding

**Pre-training**: feed large amount of unlabeled data to the model with the goal to learn general patterns
**Alignment**: adapt the model to specific task with relatively small amount of labeled data with the goal to reach desirable results

Transformer:
	Encoder: learn good embeddings
	Decoder: generate new contents
	Encoder-Decoder

Greedy Search: Emit the most likely token every step
Beam Search: Maintain the most likely beam_num hypotheses and then pick the best one
Sapling Decoding

**GPT Generative Pretrained Transformer**
# Applications
text, audio, image
# Transformer Process
**Embedding**: Input media is divided into **tokens**, which are then embedded as vectors
**Attention**: vectors communicate with each others to update the values of each other (figure out the meaning based on context)
**MLPs Multilayer Perceptrons**:
{go through Attention + MLPs multiple times}
**Unembedding**: The result vector is unembedded to a token
	**softmax** with **temperature**: logits => probabilities (of the next word to generate)
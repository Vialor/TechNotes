RAG Retrieval Augmented Generation Assessment

Dataset:
	questions: List of questions
	answers: List of generated answers
	contexts: List of contexts used for each answer (list of lists)
	ground truths: Optional list of ground truth answers
Metrics:
	生成器：
		Answer Relevance: Query Answer
		Faithfulness: Answer Context
	检索器：
		Context Precision: Query, Context, Ground Truth
		Context Recall: Context, Ground Truth
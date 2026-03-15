# 5 Steps of RAG
```python
# 1 loading
documents = SimpleDirectoryReader("<data_dir>").load_data()
# 2 indexing
index = VectorStoreIndex.from_documents(documents)
# 3 storing
index.storage_context.persist(persist_dir="<persist_dir>")
## rebuild storage context
storage_context = StorageContext.from_defaults(persist_dir="<persist_dir>")
## load index
index = load_index_from_storage(storage_context)
# 4 querying
query_engine = index.as_query_engine()
response = query_engine.query("What did the author do growing up?")
# 5 evaluation
print(response)
```
- **Loading**
	**Nodes and Documents**: A `Document` is a container around any data source - for instance, a PDF, an API output, or retrieve data from a database. A `Node` is the atomic unit of data in LlamaIndex and represents a "chunk" of a source `Document`. Nodes have metadata that relate them to the document they are in and to other nodes.
	**Connectors**: A data connector (often called a `Reader`) ingests data from different data sources and data formats into `Documents` and `Nodes`.
- **Indexing**: generate *vector embeddings* in a special database called *vector store*
- **Storing**: make the indexing persistent on disk
- **Querying**
	[**Retrievers**](https://docs.llamaindex.ai/en/stable/module_guides/querying/retriever/): A retriever defines how to efficiently retrieve relevant context from an index when given a query. Your retrieval strategy is key to the relevancy of the data retrieved and the efficiency with which it's done.
	[**Routers**](https://docs.llamaindex.ai/en/stable/module_guides/querying/router/): A router determines which retriever will be used to retrieve relevant context from the knowledge base. More specifically, the `RouterRetriever` class, is responsible for selecting one or multiple candidate retrievers to execute a query. They use a selector to choose the best option based on each candidate's metadata and the query.
	[**Node Postprocessors**](https://docs.llamaindex.ai/en/stable/module_guides/querying/node_postprocessors/): A node postprocessor takes in a set of retrieved nodes and applies transformations, filtering, or re-ranking logic to them.
	[**Response Synthesizers**](https://docs.llamaindex.ai/en/stable/module_guides/querying/response_synthesizers/): A response synthesizer generates a response from an LLM, using a user query and a given set of retrieved text chunks.
- **Evaluation**
# RAG Pipeline
## Readers: Data source --> Document
1. 
```python
from llama_index.core import Document
doc = Document(text="text")
```
3. `SimpleDirectoryReader("./data").load_data()`
	creates documents out of every file in a given directory
3. LlamaHub
4. LlamaCloud
## Transformation: Node --> Node
> chunking, extracting metadata, and embedding each chunk
## Indexing: Node --> Index
`VectorStoreIndex`
Summary Index
## Querying
Three Stages:
	- **Retrieval**: find and return the most relevant documents for your query from your `Index`
	- **Postprocessing**: `Node`s retrieved are optionally reranked, transformed, or filtered, for instance by requiring that they have specific metadata such as keywords attached.
	- **Response synthesis** is when your query, your most-relevant data and your prompt are combined and sent to your LLM to return a response.
```python
from llama_index.core import VectorStoreIndex, get_response_synthesizer
from llama_index.core.retrievers import VectorIndexRetriever
from llama_index.core.query_engine import RetrieverQueryEngine
from llama_index.core.postprocessor import SimilarityPostprocessor
# build index
index = VectorStoreIndex.from_documents(documents)
# configure retriever
retriever = VectorIndexRetriever( index=index, similarity_top_k=10, )
# configure response synthesizer
response_synthesizer = get_response_synthesizer()
# assemble query engine
query_engine = RetrieverQueryEngine(
	retriever=retriever,
	response_synthesizer=response_synthesizer,
	node_postprocessors=[SimilarityPostprocessor(similarity_cutoff=0.7)],
)
# query
response = query_engine.query("What did the author do growing up?") print(response)
```
# Agent
# Workflow
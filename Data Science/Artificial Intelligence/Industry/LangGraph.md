# LangChain Expression Language (LCEL)
>LCEL is an operation to chain Runnable objects
>	`chain = prompt | model | output_parser`
1. We pass in user input on the desired topic as `{"topic": "ice cream"}`
2. The `prompt` component takes the user input, which is then used to construct a PromptValue after using the `topic` to construct the prompt.
3. The `model` component takes the generated prompt, and passes into the OpenAI LLM model for evaluation. The generated output from the model is a `ChatMessage` object.
4. Finally, the `output_parser` component takes in a `ChatMessage`, and transforms this into a Python string, which is returned from the invoke method.

```python
RunnableLambda # a wrapper to make a function Runnable
itemgetter(<indices or keys>)(<list or dict>)`
RunnablePassthrough() # the same key value pair will be in the output
RunnableParallel

chain.get_graph().print_ascii()
```
# Components
Many components implemented the interface **Runnable**:
- invoke/ainvoke: Transforms a single input into an output.
- batch/abatch: Efficiently transforms multiple inputs into outputs.
- stream/astream: Streams output from a single input as it’s produced.
- astream_log: Streams output and selected intermediate results from an input.

| Component    | Input Type                                            | Output Type           |
| ------------ | ----------------------------------------------------- | --------------------- |
| Prompt       | Dictionary                                            | PromptValue           |
| ChatModel    | Single string, list of chat messages or a PromptValue | ChatMessage           |
| LLM          | Single string, list of chat messages or a PromptValue | String                |
| OutputParser | The output of an LLM or ChatModel                     | Depends on the parser |
| Retriever    | Single string                                         | List of Documents     |
| Tool         | Single string or dictionary, depending on the tool    | Depends on the tool   |
**Agent**: a system that use an LLM as a reasoning engine to determine which actions to take and what the inputs to those actions should be. The results of those actions can then be fed back into the agent and it determine whether more actions are needed, or whether it is okay to finish.

**Retriever**: an interface that returns a list of Documents given an unstructured query
A retriever does not need to be able to store documents, only to return (or retrieve) them. Retrievers can be created from vector stores, but are also broad enough to include [Wikipedia search](https://python.langchain.com/v0.2/docs/integrations/retrievers/wikipedia/) and [Amazon Kendra](https://python.langchain.com/v0.2/docs/integrations/retrievers/amazon_kendra_retriever/).

**Tool**: an interfaces that an agent, a chain, or a chat model / LLM can use to interact with the world.
A tool consists of the following components:
1. The name of the tool
2. A description of what the tool does
3. JSON schema of what the inputs to the tool are
4. The function to call
5. Whether the result of a tool should be returned directly to the user (only relevant for agents)
The name, description and JSON schema are provided as context to the LLM, allowing the LLM to determine how to use the tool appropriately.

Given a list of available tools and a prompt, an LLM can request that one or more tools be invoked with appropriate arguments.
Generally, when designing tools to be used by a chat model or LLM, it is important to keep in mind the following:
- Chat models that have been fine-tuned for tool calling will be better at tool calling than non-fine-tuned models.
- Non fine-tuned models may not be able to use tools at all, especially if the tools are complex or require multiple tool calls.
- Models will perform better if the tools have well-chosen names, descriptions, and JSON schemas.
- Simpler tools are generally easier for models to use than more complex tools.

**Prompt**: it takes in a dictionary of template variables and produces a `PromptValue`. A `PromptValue` is a wrapper around a completed prompt that can be passed to either an `LLM` (which takes a string as input) or `ChatModel` (which takes a sequence of messages as input)
The reason this PromptValue exists is to make it easy to switch between strings and messages.
	**prompt**: the user input into llm
	**zero-shot prompting**: user input without example
	**few-shot prompting**: user input with a few examples
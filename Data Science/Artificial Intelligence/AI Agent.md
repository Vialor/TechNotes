# Development
## Challenges
IO：流式对话和多模态
存取：会话记忆和数据持久化
交互：流程控制
## Popular Frameworks
拟人协作：简洁黑盒
	AutoGen, CrewAI
流程控制：细致可控
	Langraph, LlamaIndex
# Workflow
**Prompt chaining** decomposes a task into a sequence of steps, where each LLM call processes the output of the previous one. You can add programmatic checks  on any intermediate steps to ensure that the process is still on track.

**Routing** classifies an input and directs it to a specialized followup task. This workflow allows for separation of concerns, and building more specialized prompts.

**Parallelization**
- **Sectioning**: Breaking a task into independent subtasks run in parallel.
- **Voting:** Running the same task multiple times to get diverse outputs.

**Orchestrator-workers**: a central LLM dynamically breaks down tasks, delegates them to worker LLMs, and synthesizes their results.

**Evaluator-optimizer**: one LLM call generates a response while another provides evaluation and feedback in a loop
# Agent

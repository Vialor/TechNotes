# 定义

## 结构定义

**MCP \(Model Context Protocol\)** 是一个连接AI应用和外部系统的开源标准。包含以下项目内容：

* **MCP Specification**: 对 MCP 的规范说明，概述了客户端和服务器的实现要求

* **MCP SDKs**: 针对不同编程语言、实现了 MCP 的 SDK

* **MCP Development Tools**: 用于开发 MCP 服务器和客户端的工具，包括 **MCP Inspector**

* **MCP Reference Server Implementations**: MCP 服务器的参考实现

## 功能定义

MCP为AI应用与外部系统交流（交流数据，工具，提示词）提供了统一的对话规则。这帮助了AI应用与外部系统交流的规模化，因为外部系统和AI应用各自遵守MCP就可以了，不用专门为了彼此去做适配。

MCP的本质是在解耦，且遵循依赖倒置原则，即高层模块（AI应用）不应该依赖低层模块（外部系统），二者都应该依赖抽象。

# 工作机制

## Participants

* **MCP Host**: 协调和管理一个或多个MCP clients的AI应用本体

* **MCP Client**: MCP Host使用的一个连接器对象：维护与 MCP Server 连接，并从 MCP Server 获取上下文

* **MCP Server**: 向 MCP Client 提供上下文的程序

## Layers

Data Layer: 定义了一个基于JSON\-RPC 2.0的client\-server交流协议，包括生命周期管理，以及一些核心原语，例如 tools、resources、prompts 和 notifications。

Transport Layer: 定义了客户端与服务器之间进行数据交换的通信机制和通道，包括传输方式特定的连接建立、消息封装（message framing）和授权。

概念上，Data Layer是里层，Transport Layer是外层。

### Data Layer Protocol

MCP是一个**有状态的协议**，它需要**生命周期管理**，管理的目的是协商客户端和服务器支持的能力。

**MCP原语**定义了哪类上下文信息可以和AI应用共享，以及可执行的动作范围。

**服务器测三个核心原语**：tools, resources, prompts

&emsp;&emsp;搭配以下方法：discovery `*/list`*, *retrieval `*/get`, execute `*/call`

**客户端测原语**：sampling, elicitation, logging

&emsp;&emsp;调用模型`sampling/complete`，从用户获取信息`elicitation/request`，logging允许服务器向客户端发送日志

# 参考

[modelcontextprotocol.io](https://modelcontextprotocol.io/docs/getting-started/intro)

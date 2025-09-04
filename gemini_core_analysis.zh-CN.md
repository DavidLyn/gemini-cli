# `packages/core` 目录分析

本文档概述了 Gemini 项目中 `packages/core` 目录的代码结构。

## `packages/core/src/core`

该目录包含 Gemini 客户端的核心逻辑。

*   **`client.ts`**: 定义了 `GeminiClient` 类，这是与 Gemini API 交互的主要入口点。它管理聊天会话，包括历史记录、工具和上下文。它还处理聊天压缩以保持在令牌限制内。
*   **`geminiChat.ts`**: 定义了 `GeminiChat` 类，它是 Gemini API 聊天功能的包装器。它处理发送和接收消息、管理聊天历史记录以及在失败时重试请求。

## `packages/core/src/tools`

该目录包含 Gemini 代理可以使用的工具的实现。

*   **`tool-registry.ts`**: 定义了 `ToolRegistry` 类，负责管理代理可以使用的所有工具。它可以注册工具，从命令行或 MCP 服务器发现它们，并检索它们的函数声明。
*   **单个工具文件 (例如 `ls.ts`, `read-file.ts`)**: 每个工具都实现为一个扩展 `BaseDeclarativeTool` 的类。该工具的执行逻辑位于相应的 `ToolInvocation` 类中。

## `packages/core/src/services`

该目录包含抽象出系统级操作的服务。

*   **`fileSystemService.ts`**: 定义了 `FileSystemService` 接口和 `StandardFileSystemService` 实现。该服务提供了一种读取和写入文本文件的方法，从而抽象了底层的文件系统操作。
*   **`shellExecutionService.ts`**: 定义了 `ShellExecutionService` 类，负责执行 shell 命令。它可以使用 `node-pty` 或内置的 `child_process` 模块来运行命令，并提供了一种流式传输输出和处理进程终止的方法。

## `packages/core/src/ide`

该目录包含与 IDE 集成的逻辑。

*   **`ide-client.ts`**: 定义了 `IdeClient` 类，它是一个单例，用于管理与 IDE 的连接。它可以通过 HTTP 或 stdio 连接到 IDE，并处理与 IDE 的 MCP 服务器的通信。它还提供了在 IDE 中打开和关闭差异的方法。

## `packages/core/src/mcp`

该目录包含用于向 MCP 服务器进行身份验证的逻辑。

*   **`google-auth-provider.ts`**: 定义了 `GoogleCredentialProvider` 类，该类实现了来自 MCP SDK 的 `OAuthClientProvider` 接口。它使用 `google-auth-library` 从 Google 应用程序默认凭据 (ADC) 获取访问令牌。这允许 CLI 向 Google 服务进行身份验证，而无需用户显式登录。

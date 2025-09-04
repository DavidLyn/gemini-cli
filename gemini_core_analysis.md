# Analysis of the `packages/core` directory

This document provides a high-level overview of the code structure of the `packages/core` directory in the Gemini project.

## `packages/core/src/core`

This directory contains the core logic for the Gemini client.

*   **`client.ts`**: Defines the `GeminiClient` class, which is the main entry point for interacting with the Gemini API. It manages the chat session, including history, tools, and context. It also handles chat compression to stay within token limits.
*   **`geminiChat.ts`**: Defines the `GeminiChat` class, which is a wrapper around the Gemini API's chat functionality. It handles sending and receiving messages, managing the chat history, and retrying requests on failure.

## `packages/core/src/tools`

This directory contains the implementation of the tools that can be used by the Gemini agent.

*   **`tool-registry.ts`**: Defines the `ToolRegistry` class, which is responsible for managing all the tools that can be used by the agent. It can register tools, discover them from the command line or MCP servers, and retrieve their function declarations.
*   **Individual tool files (e.g., `ls.ts`, `read-file.ts`)**: Each tool is implemented as a class that extends `BaseDeclarativeTool`. The execution logic for the tool is in a corresponding `ToolInvocation` class.

## `packages/core/src/services`

This directory contains services that abstract away system-level operations.

*   **`fileSystemService.ts`**: Defines the `FileSystemService` interface and a `StandardFileSystemService` implementation. This service provides a way to read and write text files, abstracting the underlying file system operations.
*   **`shellExecutionService.ts`**: Defines the `ShellExecutionService` class, which is responsible for executing shell commands. It can use `node-pty` or the built-in `child_process` module to run commands, and it provides a way to stream the output and handle process termination.

## `packages/core/src/ide`

This directory contains the logic for integrating with IDEs.

*   **`ide-client.ts`**: Defines the `IdeClient` class, which is a singleton that manages the connection to an IDE. It can connect to the IDE via HTTP or stdio, and it handles communication with the IDE's MCP server. It also provides methods for opening and closing diffs in the IDE.

## `packages/core/src/mcp`

This directory contains the logic for authentication with MCP servers.

*   **`google-auth-provider.ts`**: Defines the `GoogleCredentialProvider` class, which implements the `OAuthClientProvider` interface from the MCP SDK. It uses the `google-auth-library` to get access tokens from the Google Application Default Credentials (ADC). This allows the CLI to authenticate with Google services without requiring the user to explicitly log in.

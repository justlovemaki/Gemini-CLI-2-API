<div align="center">

# Gemini-CLI-2-API 🚀

**一个能将多种大模型 API（Gemini, OpenAI, Claude...）统一封装为本地 OpenAI 兼容接口的强大代理。**

</div>

<div align="center">

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Node.js](https://img.shields.io/badge/Node.js-≥20.0.0-green.svg)](https://nodejs.org/)

[**中文**](./README.md) | [**English**](./README-EN.md)

</div>

> `GeminiCli2API` 是一个多功能、轻量化的 API 代理，旨在提供极致的灵活性和易用性。它通过一个 Node.js HTTP 服务器，将 Google Gemini (CLI 授权)、OpenAI、Claude 等多种后端 API 统一转换为标准的 OpenAI 格式接口。项目开箱即用，`npm install` 后即可直接运行，无需复杂配置。您只需在配置文件中轻松切换模型服务商，就能让任何兼容 OpenAI 的客户端或应用，通过同一个 API 地址，无缝地使用不同的大模型能力，彻底摆脱为不同服务维护多套配置和处理接口不兼容问题的烦恼。

---

## 💡 核心优势

*   ✅ **多模型统一接入**：一个接口，通吃 Gemini、OpenAI、Claude 等多种模型。通过简单的启动参数或请求头，即可在不同模型服务商之间自由切换。
*   ✅ **突破官方限制**：通过支持 Gemini CLI 的 OAuth 授权方式，有效绕过官方免费 API 的速率和配额限制，让您享受更高的请求额度和使用频率。
*   ✅ **无缝兼容 OpenAI**：提供与 OpenAI API 完全兼容的接口，让您现有的工具链和客户端（如 LobeChat, NextChat 等）可以零成本接入所有支持的模型。
*   ✅ **增强的可控性**：通过强大的日志功能，可以捕获并记录所有请求的提示词（Prompts），便于审计、调试和构建私有数据集。
*   ✅ **极易扩展**：得益于全新的模块化和策略模式设计，添加一个新的模型服务商变得前所未有的简单。

---

## 📝 项目架构

告别了过去简单的结构，我们引入了更专业、更具扩展性的设计模式，让项目脱胎换骨：

*   **`src/api-server.js`**: 🚀 **项目启动入口**
    *   作为项目的总指挥，它负责启动和管理整个 HTTP 服务，解析命令行参数，并加载所有配置。

*   **`src/adapter.js`**: 🔌 **服务适配器**
    *   采用经典的适配器模式，为每种 AI 服务（Gemini, OpenAI, Claude）创建一个统一的接口。无论后端服务如何变化，对主服务来说，调用方式都是一致的。

*   **`src/provider-strategies.js`**: 🎯 **提供商策略模式**
    *   我们为每种 API 协议（如 OpenAI、Gemini、Claude）都定义了一套策略。这套策略精确地处理了该协议下的请求解析、响应格式化、模型名称提取等所有细节，确保了协议之间的完美转换。

*   **`src/convert.js`**: 🔄 **格式转换中心**
    *   这是实现“万物皆可 OpenAI”魔法的核心。它负责在不同的 API 协议格式之间进行精确、无损的数据转换。

*   **`src/common.js`**: 🛠️ **通用工具库**
    *   存放着项目共享的常量、工具函数和通用处理器，让代码更加整洁和高效。

*   **`src/gemini/`, `src/openai/`, `src/claude/`**: 📦 **提供商实现目录**
    *   每个目录都包含了对应服务商的核心逻辑、API 调用和策略实现，结构清晰，便于您未来添加更多新的服务商。

---

### ⚠️ 目前的局限

*   暂未实现原版 Gemini CLI 的内置命令功能。配合其他客户端的mcp能力可实现相同效果。
*   多模态能力（如图片输入）尚在开发计划中 (TODO)。

---

## 🛠️ 主要功能

#### 通用功能
*   🔐 **智能认证与令牌续期**: 针对需要 OAuth 的服务（如 `gemini-cli-oauth`），首次运行将引导您通过浏览器完成授权，并能自动刷新令牌。
*   🛡️ **统一的 API Key 认证**: 所有服务均通过统一的 `Authorization: Bearer <key>` 方式进行认证，简单方便。
*   ⚙️ **高度可配置**: 可通过 `config.json` 文件或命令行参数，灵活配置监听地址、端口、API 密钥、模型提供商以及日志模式。
*   📜 **全面可控的日志系统**: 可将带时间戳的提示词日志输出到控制台或文件，并显示令牌剩余有效期。

#### OpenAI 兼容接口 (`/v1/...`)
*   🌍 **完美兼容**: 实现了 `/v1/models` 和 `/v1/chat/completions` 核心端点。
*   🔄 **自动格式转换**: 在内部自动将不同模型的请求/响应与 OpenAI 格式进行无缝转换。
*   💨 **流式传输支持**: 完全支持 OpenAI 的流式响应 (`"stream": true`)，提供打字机般的实时体验。

---

## 📦 安装指南

1.  **环境准备**:
    *   请确保您已安装 [Node.js](https://nodejs.org/) (建议版本 >= 20.0.0)。
    *   本项目已包含 `package.json` 并设置 `{"type": "module"}`，您无需手动创建。

2.  **安装依赖**:
    克隆本仓库后，在项目根目录下执行：
    ```bash
    npm install
    ```
    这将自动安装所有必要依赖。

---

## 🚀 快速开始

### 1. 配置文件 (`config.json`)

我们推荐使用 `config.json` 文件来管理您的配置，这比冗长的命令行参数更清晰。

首先，手动创建 `config.json` 文件，并填入您的配置信息。

```json
{
    "REQUIRED_API_KEY": "123456",
    "SERVER_PORT": 3000,
    "HOST": "localhost",
    "MODEL_PROVIDER": "gemini-cli-oauth",
    "OPENAI_API_KEY": "sk-your-openai-key",
    "OPENAI_BASE_URL": "https://api.openai.com/v1",
    "CLAUDE_API_KEY": "sk-ant-your-claude-key",
    "CLAUDE_BASE_URL": "https://api.anthropic.com/v1",
    "PROJECT_ID": "your-gcp-project-id",
    "PROMPT_LOG_MODE": "console"
}
```

### 2. 配置参数详解

以下是 `config.json` 文件中所有支持的参数及其详细说明：

| 参数名                          | 类型    | 描述                                                                                                                            | 默认值/可选值                                                                                             |
| ------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| `REQUIRED_API_KEY`              | string  | 用于保护您的 API 服务的密钥。客户端在请求时必须提供此密钥。                                                                     | 任意字符串, 默认为 `"123456"`                                                                             |
| `SERVER_PORT`                   | number  | 服务器监听的端口号。                                                                                                            | 任意有效端口号, 默认为 `3000`                                                                             |
| `HOST`                          | string  | 服务器监听的主机地址。`localhost` 只允许本机访问，`0.0.0.0` 允许局域网或公网访问。                                                | 默认为 `"localhost"`                                                                                      |
| `MODEL_PROVIDER`                | string  | 指定后端使用的模型服务商。这是核心配置，决定了 API 请求将转发给哪个平台。                                                         | 可选值: `"gemini-cli-oauth"`, `"openai-custom"`, `"claude-custom"`, `"openai-kiro-oauth"`                  |
| `OPENAI_API_KEY`                | string  | 当 `MODEL_PROVIDER` 为 `openai-custom` 时，需要提供您的 OpenAI API 密钥。                                                         | `null`                                                                                                    |
| `OPENAI_BASE_URL`               | string  | 当 `MODEL_PROVIDER` 为 `openai-custom` 时，可以指定 OpenAI 兼容的 API 地址。                                                      | 默认为 `"https://api.openai.com/v1"`                                                                      |
| `CLAUDE_API_KEY`                | string  | 当 `MODEL_PROVIDER` 为 `claude-custom` 时，需要提供您的 Claude API 密钥。                                                         | `null`                                                                                                    |
| `CLAUDE_BASE_URL`               | string  | 当 `MODEL_PROVIDER` 为 `claude-custom` 时，可以指定 Claude 兼容的 API 地址。                                                      | 默认为 `"https://api.anthropic.com/v1"`                                                                   |
| `GEMINI_OAUTH_CREDS_BASE64`     | string  | (Gemini-CLI 模式) 您的 Google OAuth 凭据的 Base64 编码字符串。                                                                  | `null`                                                                                                    |
| `GEMINI_OAUTH_CREDS_FILE_PATH`  | string  | (Gemini-CLI 模式) 您的 Google OAuth 凭据 JSON 文件的路径。                                                                      | `null`                                                                                                    |
| `PROJECT_ID`                    | string  | (Gemini-CLI 模式) 您的 Google Cloud 项目 ID。                                                                                   | `null`                                                                                                    |
| `SYSTEM_PROMPT_FILE_PATH`       | string  | 用于加载系统提示词的外部文件路径。                                                                                              | 默认为 `"input_system_prompt.txt"`                                                                        |
| `SYSTEM_PROMPT_MODE`            | string  | 系统提示词的应用模式。`overwrite` 会覆盖客户端的提示，`append` 会追加到客户端提示之后。                                           | 可选值: `"overwrite"`, `"append"`                                                                         |
| `PROMPT_LOG_MODE`               | string  | 请求和响应的日志记录模式。`none` 不记录，`console` 打印到控制台，`file` 保存到日志文件。                                          | 可选值: `"none"`, `"console"`, `"file"`                                                                   |
| `PROMPT_LOG_BASE_NAME`          | string  | 当 `PROMPT_LOG_MODE` 为 `file` 时，生成的日志文件的基础名称。                                                                   | 默认为 `"prompt_log"`                                                                                     |
| `REQUEST_MAX_RETRIES`           | number  | 当 API 请求失败时，自动重试的最大次数。                                                                                         | 默认为 `3`                                                                                                |
| `REQUEST_BASE_DELAY`            | number  | 自动重试之间的基础延迟时间（毫秒）。每次重试后延迟会增加。                                                                      | 默认为 `1000`                                                                                             |

### 3. 启动服务

*   **使用 `config.json` 启动** (推荐)
    ```bash
    node src/api-server.js
    ```
*   **通过命令行参数启动** (会覆盖 `config.json` 中的同名配置)
    *   **启动 OpenAI 代理**:
        ```bash
        node src/api-server.js --model-provider openai-custom --openai-api-key sk-xxx
        ```
    *   **启动 Claude 代理**:
        ```bash
        node src/api-server.js --model-provider claude-custom --claude-api-key sk-ant-xxx
        ```
    *   **监听所有网络接口并指定端口和Key** (用于 Docker 或局域网访问)
        ```bash
        node src/api-server.js --host 0.0.0.0 --port 8000 --api-key your_secret_key
        ```

*更多启动参数，请参考 `src/api-server.js` 文件顶部的注释。*

---

### 4. 调用 API

> **提示**: 如果您在无法直接访问 Google/OpenAI/Claude 服务的环境中使用，请先为您的终端设置全局 HTTP/HTTPS 代理。

所有请求都使用标准的 OpenAI 格式。

*   **列出模型**
    ```bash
    curl http://localhost:3000/v1/models \
      -H "Authorization: Bearer 123456"
    ```
*   **生成内容 (非流式)**
    ```bash
    curl http://localhost:3000/v1/chat/completions \
      -H "Content-Type: application/json" \
      -H "Authorization: Bearer 123456" \
      -d '{
        "model": "gemini-1.5-flash-latest",
        "messages": [
          {"role": "system", "content": "你是一只名叫 Neko 的猫。"},
          {"role": "user", "content": "你好，你叫什么名字？"}
        ]
      }'
    ```
*   **流式生成内容**
    ```bash
    curl http://localhost:3000/v1/chat/completions \
      -H "Content-Type: application/json" \
      -H "Authorization: Bearer 123456" \
      -d '{
        "model": "claude-3-opus-20240229",
        "messages": [
          {"role": "user", "content": "写一首关于宇宙的五行短诗"}
        ],
        "stream": true
      }'
    ```

---

## 🌟 特殊用法与进阶技巧

*   **🔌 对接任意 OpenAI 客户端**: 这是本项目的基本功能。将任何支持 OpenAI 的应用（如 LobeChat, NextChat, VS Code 插件等）的 API 地址指向本服务 (`http://localhost:3000`)，即可无缝使用所有已配置的模型。

*   **🔍 中心化请求监控与审计**: 在 `config.json` 中设置 `"PROMPT_LOG_MODE": "file"` 来捕获所有请求和响应，并保存到本地日志文件。这对于分析、调试和优化提示词，甚至构建私有数据集都至关重要。

*   **💡 动态系统提示词**:
    *   通过在 `config.json` 中设置 `SYSTEM_PROMPT_FILE_PATH` 和 `SYSTEM_PROMPT_MODE`，您可以更灵活地控制系统提示词的行为。
    *   **支持的模式**:
        *   `override`: 完全忽略客户端的系统提示词，强制使用文件中的内容。
        *   `append`: 在客户端系统提示词的末尾追加文件中的内容，实现规则的补充。
    *   这使得您可以为不同的客户端设置统一的基础指令，同时允许单个应用进行个性化扩展。

*   **🛠️ 作为二次开发基石**:
    *   **添加新模型**: 只需在 `src` 目录下创建一个新的提供商目录，实现 `ApiServiceAdapter` 接口和相应的策略，然后在 `adapter.js` 和 `common.js` 中注册即可。
    *   **响应缓存**: 对高频重复问题添加缓存逻辑，降低 API 调用，提升响应速度。
    *   **自定义内容过滤**: 在请求发送或返回前增加关键词过滤或内容审查逻辑，满足合规要求。

---

## 📄 开源许可

本项目遵循 [**GNU General Public License v3 (GPLv3)**](https://www.gnu.org/licenses/gpl-3.0) 开源许可。详情请查看根目录下的 `LICENSE` 文件。

## 🙏 致谢

本项目的开发受到了官方 Google Gemini CLI 的极大启发，并参考了Cline 3.18.0 版本 `gemini-cli.ts` 的部分代码实现。在此对 Google 官方团队和 Cline 开发团队的卓越工作表示衷心的感谢！

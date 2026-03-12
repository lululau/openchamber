# 术语表和概念索引

本文档提供 OpenChamber 项目中专业术语的中文解释和交叉引用，帮助不熟悉 AI/LLM 领域的开发者快速理解项目中的概念。

## 目录

- [A](#a) - [B](#b) - [C](#c) - [D](#d) - [E](#e) - [F](#f) - [G](#g) - [H](#h) - [I](#i) - [J](#j) - [K](#k) - [L](#l) - [M](#m) - [N](#n) - [O](#o) - [P](#p) - [Q](#q) - [R](#r) - [S](#s) - [T](#t) - [U](#u) - [V](#v) - [W](#w) - [X](#x) - [Y](#y) - [Z](#z)
- [拼音索引](#拼音索引)

---

## A

### Agent（AI 代理）

AI 代理是一种能够自主执行任务、与环境和工具交互的智能实体。在 OpenChamber 中，代理可以：

- 并行运行多个实例进行代码生成
- 使用工具（如文件操作、Git 操作、终端命令）
- 进行推理和规划
- 与用户进行多轮对话

**相关概念：** [Multi-Agent](#multi-agent多代理)、[Tool Call](#tool-call工具调用)、[Reasoning](#reasoning推理)

---

### API（Application Programming Interface）

应用程序编程接口，定义软件组件之间交互的规范。OpenChamber 使用多种 API：

- **OpenCode API**：与 OpenCode 服务器通信的核心接口
- **GitHub API**：通过 Octokit 库进行 GitHub 操作
- **Runtime APIs**：跨运行时（Web/Desktop/VS Code）的统一接口层

**相关概念：** [REST](#restrest)、[SSE](#sseserver-sent-events)、[WebSocket](#websocket)

**参考文档：** [API 参考](./06-api-reference.md)

---

### Auth Flow（认证流程）

用户身份验证的完整流程。OpenChamber 支持多种认证方式：

- **GitHub Device Flow**：通过设备码进行 GitHub OAuth 认证
- **UI Auth**：基于密码的 UI 会话认证（带速率限制）
- **Provider Auth**：AI 服务提供商的 API 密钥认证

**相关概念：** [OAuth](#oauthoauth)、[Session](#session会话)

---

## B

### Branch（分支）

Git 中的独立开发线。OpenChamber 支持完整的分支操作：

- 创建、切换、重命名分支
- 查看分支状态和差异
- 分支与工作树的关联管理

**相关概念：** [Worktree](#worktree工作树)、[Merge](#merge合并)、[Rebase](#rebase变基)

---

### Bun

现代 JavaScript 运行时和包管理器，OpenChamber 的主要运行环境。

- 版本要求：^1.3.5
- 支持原生的 TypeScript 执行
- 比 Node.js 更快的启动速度

**相关概念：** [Node.js](#nodejs)、[Runtime](#runtime运行时)

**参考文档：** [技术栈](./02-tech-stack.md)

---

## C

### Cloudflare Tunnel

Cloudflare 提供的内网穿透服务，OpenChamber 用于实现远程访问：

- **Quick Tunnel**：临时隧道，自动生成域名
- **Named Tunnel**：持久化隧道，使用自定义域名
- 支持 QR 码快速入网

**相关概念：** [PWA](#pwaprogressive-web-app)、[Remote Access](#remote-access远程访问)

---

### Commit（提交）

Git 中的代码变更记录点。OpenChamber 提供：

- AI 生成提交信息
- 支持追加所有文件或指定文件
- Gitmoji 表情符号支持

**相关概念：** [Git](#gitgit)、[Push](#push推送)、[PR](#prpull-request)

---

### Config（配置）

系统或项目的设置信息。OpenChamber 使用分层配置系统：

- **User Config**：`~/.config/opencode/opencode.json`
- **Project Config**：项目目录下的 `.opencode/opencode.json`
- **Custom Config**：通过 `OPENCODE_CONFIG` 环境变量指定

配置优先级：Custom > Project > User

**相关概念：** [Settings](#settings设置)、[Provider](#provider提供商)

---

## D

### Diff（差异）

文件或代码变更的对比视图。OpenChamber 的差异查看器特性：

- 支持 Inline（内联）和 Side-by-Side（并排）模式
- 语法高亮显示
- 大文件延迟加载
- 基于 Pierre 差异视图库

**相关概念：** [Merge](#merge合并)、[Conflict](#conflict冲突)

---

### Desktop（桌面应用）

基于 Tauri 的 macOS 原生应用，是 Web UI 的轻量级外壳：

- 原生菜单、对话框、通知
- SSH 远程实例连接
- 项目动作管理
- 多窗口支持

**相关概念：** [Tauri](#tauritauri)、[Runtime](#runtime运行时)

**参考文档：** [架构设计](./03-architecture.md)

---

## E

### Event（事件）

系统中发生的动作或状态变化。OpenChamber 通过 SSE 流式传输事件：

- 消息更新事件
- 代理状态事件
- 系统通知事件

**相关概念：** [SSE](#sseserver-sent-events)、[Stream](#stream流)

---

## F

### Fork（复刻）

GitHub 仓库的副本，用于独立开发。OpenChamber 支持：

- 从 Fork 创建 PR
- 多远程推送（origin + upstream）
- Fork 感知的 PR 创建流程

**相关概念：** [PR](#prpull-request)、[Remote](#remote远程仓库)

---

## G

### Git

分布式版本控制系统。OpenChamber 集成了完整的 Git 操作：

- 状态查看、暂存、提交
- 分支管理、合并、变基
- 推送、拉取、远程操作
- 工作树管理

**相关概念：** [GitHub](#githubgithub)、[Worktree](#worktree工作树)

**参考文档：** [API 参考 - Git](./06-api-reference.md#git)

---

### GitHub

代码托管和协作平台。OpenChamber 深度集成 GitHub 功能：

- Issue 和 PR 管理
- 状态检查（Checks）
- 多账户 OAuth 认证
- AI 生成 PR 描述

**相关概念：** [PR](#prpull-request)、[OAuth](#oauthoauth)、[Check Run](#check-run检查运行)

**参考文档：** [API 参考 - GitHub](./06-api-reference.md#github)

---

## H

### Hook（钩子）

在特定事件发生时自动触发的函数。在 OpenChamber 中：

- **React Hooks**：如 `useEventStream`、`useChatScrollManager`
- **Session Hooks**：`useSessionActions`、`useProjectSessionLists`
- **Custom Hooks**：封装可复用的状态逻辑

**相关概念：** [React](#reactreact)、[Store](#store状态存储)

---

## I

### Issue（议题）

GitHub 上的问题追踪单元。OpenChamber 支持：

- 从 Issue 启动会话（自动附带上下文）
- 查看 Issue 列表和详情
- Issue 评论管理

**相关概念：** [PR](#prpull-request)、[Session](#session会话)

---

## J

### JSON（JavaScript Object Notation）

轻量级数据交换格式。OpenChamber 广泛使用 JSON：

- 配置文件格式（`opencode.json`）
- API 请求/响应体
- 主题定义文件

**相关概念：** [Config](#config配置)、[Theme](#theme主题)

---

## K

### Keybinding（键盘快捷键）

自定义键盘组合键，用于快速执行操作。OpenChamber 支持：

- 可配置的聊天、面板、服务快捷键
- 跨运行时一致的用户体验
- 自定义快捷键配置

**相关概念：** [Settings](#settings设置)、[Runtime](#runtime运行时)

---

## L

### LLM（Large Language Model）

大型语言模型，AI 代理的核心技术。OpenChamber 支持多个 LLM 提供商：

- OpenAI（GPT-4、GPT-3.5）
- Anthropic（Claude）
- 其他兼容 OpenAI API 的服务

**相关概念：** [Provider](#provider提供商)、[Agent](#agentai-代理)、[Token](#token令牌)

---

### Log（日志）

系统运行记录。OpenChamber 支持日志操作：

- Git 提交日志查看
- 诊断日志下载
- Tauri 日志插件

---

## M

### MCP（Model Context Protocol）

模型上下文协议，用于 AI 代理与外部服务通信的标准协议。OpenChamber 支持：

- MCP 服务器管理
- 下拉选择器界面
- 动态服务器发现

**相关概念：** [Agent](#agentai-代理)、[Tool](#tool工具)

---

### Merge（合并）

将一个分支的变更整合到另一个分支。OpenChamber 支持：

- 标准合并操作
- 合并冲突检测和解决
- 合并状态详情查看

**相关概念：** [Rebase](#rebase变基)、[Conflict](#conflict冲突)、[Branch](#branch分支)

---

### Message（消息）

对话中的一个交流单元。OpenChamber 消息类型：

- **User Message**：用户输入
- **Assistant Message**：AI 代理回复
- **Tool Message**：工具执行结果
- **System Message**：系统提示

每条消息由多个 Part（部分）组成，如文本、工具调用、推理等。

**相关概念：** [Part](#part部分)、[Session](#session会话)、[Stream](#stream流)

---

### Multi-Agent（多代理）

同时运行多个 AI 代理实例。OpenChamber 的多代理特性：

- 从单个提示词并行运行
- 使用独立工作树隔离
- 并排比较不同方案
- Agent Manager 管理界面

**相关概念：** [Agent](#agentai-代理)、[Worktree](#worktree工作树)、[Parallel Run](#parallel-run并行运行)

---

## N

### Notification（通知）

系统向用户传递信息的方式。OpenChamber 支持：

- 原生桌面通知（Tauri）
- Web Push 通知（PWA）
- 代理完成通知
- 可选的文本摘要

**相关概念：** [PWA](#pwaprogressive-web-app)、[Desktop](#desktop桌面应用)

---

## O

### OAuth（Open Authorization）

开放授权标准，用于安全的第三方授权。OpenChamber 使用：

- **GitHub Device Flow**：设备码授权流程
- 多账户支持
- 作用域（Scope）管理

**相关概念：** [Auth Flow](#auth-flow认证流程)、[GitHub](#githubgithub)

---

### OpenCode

AI 编码助手 CLI 工具，OpenChamber 的核心依赖：

- 提供代码生成、审查、重构功能
- 通过 HTTP API 和 SSE 与 UI 通信
- 支持技能（Skills）和命令（Commands）扩展
- 配置文件：`~/.config/opencode/opencode.json`

**相关概念：** [SDK](#sdksoftware-development-kit)、[Agent](#agentai-代理)

**参考文档：** [项目概述](./01-project-overview.md)

---

### OpenChamber

OpenCode 的富交互式用户界面，提供桌面端、浏览器端和 VS Code 扩展。

**相关概念：** [OpenCode](#opencodeopencode)、[Runtime](#runtime运行时)

**参考文档：** [项目概述](./01-project-overview.md)

---

## P

### Part（部分）

消息的组成单元。OpenChamber 中的 Part 类型：

- **TextPart**：文本内容
- **ToolPart**：工具调用及结果
- **ReasoningPart**：推理过程
- **JustificationBlock**：理由说明块

**相关概念：** [Message](#message消息)、[Tool Call](#tool-call工具调用)

---

### PR（Pull Request）

拉取请求，GitHub 上的代码审查和合并机制。OpenChamber 的 PR 功能：

- AI 生成 PR 标题和描述
- 状态检查（Checks）查看
- 文件变更和差异查看
- 合并操作（Merge/Squash/Rebase）
- 从 Fork 创建 PR

**相关概念：** [Issue](#issue议题)、[Commit](#commit提交)、[Merge](#merge合并)

**参考文档：** [API 参考 - GitHub PR](./06-api-reference.md#pull-request)

---

### Parallel Run（并行运行）

同时执行多个独立任务。OpenChamber 支持：

- 多代理并行运行
- 独立工作树隔离
- 并排结果比较

**相关概念：** [Multi-Agent](#multi-agent多代理)、[Worktree](#worktree工作树)

---

### Provider（提供商）

AI 服务的提供商。OpenChamber 支持多个提供商：

- OpenAI
- Anthropic
- 其他兼容 OpenAI API 的服务

每个提供商可能有多个模型，支持 API 密钥认证。

**相关概念：** [Model](#model模型)、[LLM](#llmlarge-language-model)、[Auth](#auth-flow认证流程)

---

### PWA（Progressive Web App）

渐进式 Web 应用，可安装的 Web 应用。OpenChamber Web 支持作为 PWA：

- 可安装到桌面或主屏幕
- 离线功能支持
- 项目感知的命名
- 后台通知

**相关概念：** [Cloudflare Tunnel](#cloudflare-tunnel)、[Notification](#notification通知)

---

## Q

### Quota（配额）

资源使用限制。OpenChamber 的配额管理：

- 多提供商配额跟踪
- 使用量和成本统计
- 节奏和预测指标
- 速率限制保护

**相关概念：** [Token](#token令牌)、[Provider](#provider提供商)

---

## R

### React

用于构建用户界面的 JavaScript 库。OpenChamber 使用：

- React 19.1.1（最新版本）
- 函数组件和 Hooks
- React Compiler 优化

**相关概念：** [Component](#component组件)、[Hook](#hook钩子)、[TypeScript](#typescripttypescript)

**参考文档：** [技术栈](./02-tech-stack.md)

---

### Reasoning（推理）

AI 代理的思考和规划过程。OpenChamber 支持：

- 推理轨迹显示
- 可配置的推理可见性
- 推理和输出分离

**相关概念：** [Agent](#agentai-代理)、[Thinking](#thinking思考)

---

### Rebase（变基）

将一系列提交移动到新的基础提交上。OpenChamber 支持：

- 变基操作
- 变基冲突处理
- 继续和中止变基

**相关概念：** [Merge](#merge合并)、[Branch](#branch分支)、[Conflict](#conflict冲突)

---

### Remote（远程仓库）

Git 中远程版本的仓库。OpenChamber 支持：

- 多远程管理
- 推送/拉取操作
- 远程 URL 配置

**相关概念：** [Push](#push推送)、[Pull](#pull拉取)、[Fork](#fork复刻)

---

### REST（Representational State Transfer）

表述性状态转移，一种 API 设计风格。OpenChamber 的 REST API：

- 统一的 `/api` 前缀
- 标准的 HTTP 方法（GET、POST、PUT、DELETE）
- JSON 请求/响应格式

**相关概念：** [API](#apiapplication-programming-interface)、[SSE](#sseserver-sent-events)

**参考文档：** [API 参考](./06-api-reference.md)

---

### Runtime（运行时）

代码执行的软件环境。OpenChamber 支持三种运行时：

- **Web**：浏览器 + Express 服务器
- **Desktop**：Tauri macOS 应用
- **VS Code**：VS Code 扩展 + Webview

**相关概念：** [Tauri](#tauritauri)、[Web Server](#web-serverweb-服务器)

**参考文档：** [架构设计](./03-architecture.md)

---

## S

### SDK（Software Development Kit）

软件开发工具包。OpenChamber 使用：

- **@opencode-ai/sdk**：OpenCode 官方 SDK
- 版本：^1.2.20
- 提供 Session、Message、Part 等类型定义

**相关概念：** [OpenCode](#opencodeopencode)、[TypeScript](#typescripttypescript)

---

### Session（会话）

一次完整的对话交互单元。OpenChamber 会话特性：

- 可分支的时间线（`/undo`、`/redo`）
- 会话文件夹和子文件夹
- 草稿持久化
- 会话自动清理

**相关概念：** [Message](#message消息)、[Timeline](#timeline时间线)、[Branch](#branch分支)

---

### Settings（设置）

用户可配置的应用程序选项。OpenChamber 设置类别：

- 主题和外观
- 键盘快捷键
- Git 和 GitHub
- 通知
- 会话保留
- 内存限制

**相关概念：** [Config](#config配置)、[Theme](#theme主题)、[Keybinding](#keybinding键盘快捷键)

---

### SSE（Server-Sent Events）

服务器推送事件，一种服务器到客户端的单向实时通信技术。OpenChamber 使用：

- `/api/event` 端点
- `/api/global/event` 端点
- 流式事件传输

**相关概念：** [WebSocket](#websocket)、[Stream](#stream流)、[REST](#restrest)

---

### Skill（技能）

OpenCode 的可扩展功能单元。OpenChamber 支持：

- 技能目录浏览
- 用户和项目级别安装
- 本地技能管理
- ClawdHub 集成

**相关概念：** [Agent](#agentai-代理)、[MCP](#mcpmodel-context-protocol)、[Command](#command命令)

---

### Stash（暂存）

临时保存 Git 工作目录变更。OpenChamber 支持：

- 创建暂存（含未跟踪文件）
- 恢复暂存内容

**相关概念：** [Git](#gitgit)、[Commit](#commit提交)

---

### State（状态）

应用程序在特定时刻的数据。OpenChamber 使用：

- **Zustand**：轻量级状态管理
- 40+ 状态存储
- Persist 中间件用于本地存储持久化

**相关概念：** [Store](#store状态存储)、[Zustand](#zustandzustand)

---

### Store（状态存储）

存放应用状态的容器。OpenChamber 的 Store 类型：

- `useChatStore`：聊天状态
- `useUIStore`：UI 状态
- `useProjectStore`：项目状态
- `useMultiRunStore`：多代理运行状态
- `useQuotaStore`：配额状态

**相关概念：** [State](#state状态)、[Zustand](#zustandzustand)、[Hook](#hook钩子)

---

### Stream（流）

连续的数据传输。OpenChamber 中的流：

- 事件流（SSE）
- 消息流（流式 Markdown）
- 终端流（WebSocket）

**相关概念：** [SSE](#sseserver-sent-events)、[WebSocket](#websocket)、[Message](#message消息)

---

### T

### Tailwind CSS

实用优先的 CSS 框架。OpenChamber 使用：

- Tailwind v4.0.0
- 自定义设计系统
- 主题变量系统

**相关概念：** [Theme](#theme主题)、[CSS](#csscss)

**参考文档：** [技术栈](./02-tech-stack.md)

---

### Tauri

跨平台桌面应用框架，使用 Rust 后端和 Web 前端。OpenChamber Desktop：

- Tauri v2.9.4
- 轻量级外壳（主要功能在 Web 服务器）
- 插件：dialog、log、shell、notification、updater

**相关概念：** [Desktop](#desktop桌面应用)、[Rust](#rustrust)

**参考文档：** [技术栈](./02-tech-stack.md)

---

### Theme（主题）

应用程序的视觉风格。OpenChamber 支持：

- 18+ 内置主题（浅色/深色变体）
- 自定义 JSON 主题文件
- 热重载（无需重启）
- 主题变量系统

**相关概念：** [Settings](#settings设置)、[Tailwind CSS](#tailwind-css)

**参考文档：** [自定义主题指南](../../CUSTOM_THEMES.md)

---

### Thinking（思考）

AI 代理的内部推理过程。在 OpenChamber 中：

- 可显示/隐藏推理轨迹
- 推理块（ReasoningPart）渲染
- 推理与最终输出分离

**相关概念：** [Reasoning](#reasoning推理)、[Agent](#agentai-代理)

---

### Timeline（时间线）

对话的历史记录按时间顺序的展示。OpenChamber 的可分支时间线：

- `/undo`：撤销到上一个轮次
- `/redo`：重做已撤销的轮次
- 从任意轮次创建分支
- 并行探索不同方案

**相关概念：** [Session](#session会话)、[Branch](#branch分支)、[Message](#message消息)

---

### Token（令牌）

LLM 处理文本的基本单位。OpenChamber 的令牌管理：

- 使用量和成本跟踪
- 输入/输出/缓存令牌分类
- 配额限制和预测
- 按提供商统计

**相关概念：** [LLM](#llmlarge-language-model)、[Quota](#quota配额)、[Provider](#provider提供商)

---

### Tool（工具）

AI 代理可以调用的外部功能。OpenChamber 工具类型：

- 文件系统操作
- Git 操作
- 终端命令执行
- GitHub API 调用
- 浏览器操作

**相关概念：** [Agent](#agentai-代理)、[Tool Call](#tool-call工具调用)、[MCP](#mcpmodel-context-protocol)

---

### Tool Call（工具调用）

AI 代理调用工具的动作。OpenChamber 的工具调用：

- 可配置的扩展级别
- 权限请求流程
- 工具输出对话框
- 流式工具结果

**相关概念：** [Tool](#tool工具)、[Agent](#agentai-代理)、[Permission](#permission权限)

---

### TypeScript

JavaScript 的类型安全超集。OpenChamber 使用：

- TypeScript 5.8.3
- 严格模式启用
- Zod 模式验证
- 全面类型定义

**相关概念：** [React](#reactreact)、[Zod](#zodzod)

**参考文档：** [技术栈](./02-tech-stack.md)

---

## U

### Undo/Redo（撤销/重做）

导航会话历史的能力。OpenChamber 实现：

- `/undo`：撤销到上一个轮次
- `/redo`：重做已撤销的轮次
- 基于分支的时间线

**相关概念：** [Timeline](#timeline时间线)、[Session](#session会话)

---

## V

### VS Code（Visual Studio Code）

微软开发的代码编辑器。OpenChamber 提供：

- 官方扩展
- 编辑器内集成
- 文件跳转功能
- Agent Manager 面板

**相关概念：** [Extension](#extension扩展)、[Runtime](#runtime运行时)

**参考文档：** [技术栈 - VS Code 扩展](./02-tech-stack.md#vs-code-扩展)

---

## W

### WebSocket

全双工通信协议。OpenChamber 用于：

- 终端输入（`/api/terminal/input-ws`）
- 自定义二进制协议
- 控制帧解析

**相关概念：** [SSE](#sseserver-sent-events)、[Terminal](#terminal终端)

---

### Web Server（Web 服务器）

提供 Web 应用和 API 的服务器。OpenChamber 使用：

- Express 框架 v5.1.0
- 端口默认 3001（可通过 `OPENCHAMBER_PORT` 配置）
- 模块化服务器库

**相关概念：** [API](#apiapplication-programming-interface)、[Express](#exprexpress)

**参考文档：** [架构设计](./03-architecture.md)

---

### Worktree（工作树）

Git 的多分支并发工作功能。OpenChamber 深度集成：

- 每个分支独立工作目录
- 会话隔离和合并
- PR 工作流支持
- 冲突处理

**相关概念：** [Branch](#branch分支)、[Session](#session会话)、[Merge](#merge合并)

**参考文档：** [API 参考 - Git Worktree](./06-api-reference.md#git-worktree)

---

## Z

### Zod

TypeScript 优先的模式验证库。OpenChamber 使用：

- 版本 4.3.6
- 运行时类型验证
- API 响应验证

**相关概念：** [TypeScript](#typescripttypescript)、[Validation](#validation验证)

---

### Zustand

轻量级状态管理库。OpenChamber 使用：

- 版本 5.0.8
- 40+ 状态存储
- Persist 中间件用于本地存储

**相关概念：** [Store](#store状态存储)、[State](#state状态)

---

## 其他

### `/plan` 命令

进入计划模式，允许用户和 AI 代理协作制定实现计划。

**相关概念：** [Agent](#agentai-代理)、[Session](#session会话)

---

### `/build` 命令

执行计划模式中制定的实现步骤。

**相关概念：** [Agent](#agentai-代理)、[Plan](#plan计划模式)

---

### `/undo` 命令

撤销会话到上一个轮次。

**相关概念：** [Timeline](#timeline时间线)、[Session](#session会话)

---

### `/redo` 命令

重做已撤销的会话轮次。

**相关概念：** [Timeline](#timeline时间线)、[Session](#session会话)

---

### `!` 前缀（Shell 模式）

在聊天输入中添加 `!` 前缀执行 Shell 命令，结果内联显示。

**相关概念：** [Terminal](#terminal终端)、[Command](#command命令)

---

### Conflict（冲突）

当 Git 无法自动合并变更时发生。OpenChamber 支持：

- 冲突检测
- 冲突详情查看
- 冲突解决指导

**相关概念：** [Merge](#merge合并)、[Rebase](#rebase变基)、[Git](#gitgit)

---

### Monorepo（单体仓库）

在单个代码仓库中管理多个相关项目。OpenChamber 结构：

- `packages/ui/`：共享 UI 组件
- `packages/web/`：Web 应用和服务器
- `packages/desktop/`：桌面应用
- `packages/vscode/`：VS Code 扩展

**相关概念：** [Workspace](#workspace工作区)、[Package](#package包)

---

### Pinyin Index

本术语表的中文拼音索引：

- **A**：Agent、API、Auth Flow
- **B**：Bun、Branch
- **C**：Cloudflare Tunnel、Commit、Config
- **D**：Diff、Desktop
- **E**：Event
- **F**：Fork
- **G**：Git、GitHub
- **H**：Hook
- **I**：Issue
- **J**：JSON
- **K**：Keybinding
- **L**：LLM、Log
- **M**：MCP、Merge、Message、Multi-Agent
- **N**：Notification
- **O**：OAuth、OpenCode、OpenChamber
- **P**：Part、PR、Parallel Run、Provider、PWA
- **Q**：Quota
- **R**：React、Reasoning、Rebase、Remote、REST、Runtime
- **S**：SDK、Session、Settings、SSE、Skill、Stash、State、Store、Stream
- **T**：Tailwind CSS、Tauri、Theme、Thinking、Timeline、Token、Tool、Tool Call、TypeScript
- **U**：Undo/Redo
- **V**：VS Code
- **W**：WebSocket、Web Server、Worktree
- **Z**：Zod、Zustand

---

## 相关文档

- [快速入门](./00-quick-start.md) - 新手 5 分钟上手指南
- [项目概述](./01-project-overview.md) - 项目定位和核心功能
- [技术栈](./02-tech-stack.md) - 技术选型和版本要求
- [架构设计](./03-architecture.md) - 系统架构和模块设计
- [代码结构](./04-code-structure.md) - 目录组织和关键文件
- [开发设置](./05-development-setup.md) - 开发环境配置
- [API 参考](./06-api-reference.md) - 接口文档
- [贡献指南](./07-contributing.md) - 如何参与贡献

# OpenChamber 中文文档索引

欢迎来到 OpenChamber 中文文档中心。OpenChamber 是一个为 [OpenCode](https://opencode.ai) 打造的丰富用户界面，支持桌面端、浏览器和移动端使用。

## 关于 OpenChamber

OpenChamber 是一个独立项目，非 OpenCode 官方团队开发。它提供了一个现代化的用户界面，让你可以：

- 在任何地方使用 —— 通过 Cloudflare 隧道和二维码快速连接
- 可分支的聊天时间线 —— 使用 `/undo`、`/redo` 从任意对话轮次分叉探索
- GitHub 原生工作流 —— 从 Issue 和 PR 直接启动会话，在应用内审查代码
- 多 Agent 并行运行 —— 在隔离的工作树中安全地并排比较不同方案
- 连接远程机器 —— 桌面应用支持通过 SSH 连接远程 OpenChamber 实例

## 推荐阅读顺序

### 对于新用户

如果你想快速上手 OpenChamber，建议按以下顺序阅读：

1. **[快速入门](00-quick-start.md)** ⏱️ 5 分钟
   - 安装和连接 OpenChamber
   - 运行第一个 Hello World 示例
   - 常见问题快速排查

2. **[项目概览](01-project-overview.md)** 📖 10 分钟
   - 了解 OpenChamber 是什么
   - 核心功能介绍
   - 支持的运行时（桌面/Web/VS Code）

3. **[术语表](08-glossary.md)** 📚 随时查阅
   - AI/LLM 领域术语
   - Git/GitHub 相关概念
   - OpenChamber 特有术语

### 对于开发者

如果你想参与 OpenChamber 的开发，建议按以下顺序阅读：

1. **[快速入门](00-quick-start.md)** ⏱️ 5 分钟
2. **[项目概览](01-project-overview.md)** 📖 10 分钟
3. **[技术栈](02-tech-stack.md)** 🛠️ 15 分钟
4. **[架构设计](03-architecture.md)** 🏗️ 30 分钟
5. **[代码结构](04-code-structure.md)** 📁 20 分钟
6. **[开发环境搭建](05-development-setup.md)** 💻 15 分钟
7. **[API 参考](06-api-reference.md)** 🔌 随时查阅
8. **[贡献指南](07-contributing.md)** 🤝 必读

### 对于 API 使用者

如果你只想了解和使用 OpenChamber 的 API：

1. **[项目概览](01-project-overview.md)** 📖 10 分钟
2. **[架构设计](03-architecture.md)** 🏗️ 30 分钟
3. **[API 参考](06-api-reference.md)** 🔌 必读
4. **[术语表](08-glossary.md)** 📚 随时查阅

## 文档目录

| 文档 | 描述 | 预计阅读时间 |
|------|------|--------------|
| [快速入门](00-quick-start.md) | 5 分钟上手指南，包含安装、连接和 Hello World 示例 | 5 分钟 |
| [项目概览](01-project-overview.md) | 项目介绍、核心功能、运行时支持和设计理念 | 10 分钟 |
| [技术栈](02-tech-stack.md) | 完整的技术栈介绍，包含前端、后端、桌面端技术选型 | 15 分钟 |
| [架构设计](03-architecture.md) | 系统架构、模块关系、数据流和通信机制详解 | 30 分钟 |
| [代码结构](04-code-structure.md) | 代码组织结构、关键文件位置和设计模式 | 20 分钟 |
| [开发环境搭建](05-development-setup.md) | 开发环境配置、依赖安装和开发模式说明 | 15 分钟 |
| [API 参考](06-api-reference.md) | REST API、WebSocket 和 SSE 端点完整参考 | 随时查阅 |
| [贡献指南](07-contributing.md) | 代码规范、提交流程和 PR 指南 | 15 分钟 |
| [术语表](08-glossary.md) | AI/LLM、Git/GitHub 和项目相关术语索引 | 随时查阅 |

## 快速链接

### 按主题查找

- **安装与配置**：[快速入门](00-quick-start.md) · [开发环境搭建](05-development-setup.md)
- **理解项目**：[项目概览](01-project-overview.md) · [技术栈](02-tech-stack.md) · [架构设计](03-architecture.md)
- **代码贡献**：[代码结构](04-code-structure.md) · [开发环境搭建](05-development-setup.md) · [贡献指南](07-contributing.md)
- **API 使用**：[API 参考](06-api-reference.md) · [架构设计](03-architecture.md)
- **参考查询**：[术语表](08-glossary.md)

### 常见任务

| 任务 | 相关文档 |
|------|----------|
| 我刚听说 OpenChamber，想快速了解 | [快速入门](00-quick-start.md) → [项目概览](01-project-overview.md) |
| 我想为项目贡献代码 | [开发环境搭建](05-development-setup.md) → [贡献指南](07-contributing.md) |
| 我想了解 OpenChamber 的工作原理 | [架构设计](03-architecture.md) → [代码结构](04-code-structure.md) |
| 我想开发插件或扩展功能 | [API 参考](06-api-reference.md) · [术语表 - Skill](08-glossary.md#skill) |
| 我看到不懂的术语 | [术语表](08-glossary.md) |
| 我想了解技术栈选型 | [技术栈](02-tech-stack.md) |

## 项目信息

- **GitHub**: [https://github.com/btriapitsyn/openchamber](https://github.com/btriapitsyn/openchamber)
- **OpenCode**: [https://opencode.ai](https://opencode.ai)
- **Discord**: [加入社区](https://discord.gg/ZYRSdnwwKA)
- **许可证**: MIT

## 文档反馈

如果你发现文档有任何问题或有改进建议，欢迎：

1. 在 GitHub 上提 Issue 或 PR
2. 在 Discord 社区中反馈
3. 直接参与文档改进

---

**提示**: 文档内容基于实际代码库分析编写，如有与代码不符之处，请以实际代码为准。

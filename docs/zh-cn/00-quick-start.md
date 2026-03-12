# 快速入门指南

欢迎来到 OpenChamber！本指南将帮助你在 **5 分钟内** 完成第一个 Hello World 级别的实践。

## 目录

- [什么是 OpenChamber？](#什么是-openchamber)
- [5 分钟快速体验](#5-分钟快速体验)
- [Hello World 示例](#hello-world-示例)
- [预期结果](#预期结果)
- [常见问题](#常见问题)
- [下一步学习](#下一步学习)

---

## 什么是 OpenChamber？

**OpenChamber** 是 [OpenCode](https://opencode.ai) 的富交互式用户界面，让你可以在桌面、浏览器和 VS Code 中使用 AI 辅助编程。

简单来说：
- **OpenCode** = AI 编程助手的核心引擎
- **OpenChamber** = 漂亮、强大的用户界面

---

## 5 分钟快速体验

### 前置条件

- **Node.js 20+** 已安装
- **OpenCode CLI** 已安装（[安装指南](https://opencode.ai)）
- 5 分钟空闲时间 ⏱️

### 第一步：安装 OpenChamber（2 分钟）

#### macOS 用户 - 下载桌面应用

```bash
# 访问 Releases 页面下载
# https://github.com/btriapitsyn/openchamber/releases

# 或使用 Homebrew（如果有）
brew install --cask openchamber
```

#### Web 用户 - 使用安装脚本

```bash
# 一键安装
curl -fsSL https://raw.githubusercontent.com/btriapitsyn/openchamber/main/scripts/install.sh | bash

# 启动服务
openchamber --ui-password my-password --daemon
```

#### VS Code 用户 - 安装扩展

```bash
# 在 VS Code 中搜索 "OpenChamber" 扩展并安装
# 或访问
# https://marketplace.visualstudio.com/items?itemName=fedaykindev.openchamber
```

### 第二步：连接到 OpenCode（1 分钟）

OpenChamber 需要连接到运行中的 OpenCode 服务器。

#### 启动 OpenCode

```bash
# 在你的项目目录中启动 OpenCode
cd your-project
opencode
```

OpenCode 默认运行在 `localhost:4096`。

#### 连接 OpenChamber

**桌面应用**：自动连接到本地 OpenCode 实例

**Web 应用**：访问 `http://localhost:3001`（默认端口）

**VS Code 扩展**：点击侧边栏的 OpenChamber 图标

### 第三步：发送第一条消息（2 分钟）

在聊天框中输入：

```
你好！请帮我创建一个简单的 Hello World 程序。
```

按下 Enter 发送，AI 将开始工作！

---

## Hello World 示例

### 示例 1：创建 Python Hello World

**在聊天中输入：**

```
请创建一个名为 hello.py 的文件，打印 "Hello, OpenChamber!"
```

**AI 会执行以下操作：**

1. 创建 `hello.py` 文件
2. 写入 Python 代码
3. 显示文件预览

**预期代码内容：**

```python
# hello.py
print("Hello, OpenChamber!")
```

### 示例 2：创建 React Hello World

**在聊天中输入：**

```
创建一个 React 组件 HelloWorld.jsx，显示 "Hello, OpenChamber!"
```

**预期代码内容：**

```jsx
// HelloWorld.jsx
export function HelloWorld() {
  return <div>Hello, OpenChamber!</div>;
}
```

### 示例 3：使用 Git 工作流

**在聊天中输入：**

```
请帮我把刚才创建的 hello.py 文件提交到 Git
```

**AI 会执行以下操作：**

1. 显示当前的 Git 状态
2. 将 `hello.py` 添加到暂存区
3. 创建提交（带 AI 生成的提交信息）
4. 询问是否需要推送到远程

---

## 预期结果

### 成功连接后，你将看到：

```
┌─────────────────────────────────────────┐
│  OpenChamber                             │
├─────────────────────────────────────────┤
│  ┌─────────────────────────────────┐   │
│  │ 聊天区域                          │   │
│  │                                  │   │
│  │ AI: 你好！我是你的 AI 编程助手。  │   │
│  │                                  │   │
│  │ [输入你的问题...]                │   │
│  └─────────────────────────────────┘   │
│                                          │
│  ┌─────┐ ┌─────┐ ┌─────┐              │
│  │ 文件 │ │ Git │ │设置 │              │
│  └─────┘ └─────┘ └─────┘              │
└─────────────────────────────────────────┘
```

### 创建文件后，你将看到：

```
✓ 创建文件: hello.py
✓ 写入内容: 2 行
✓ 文件预览已显示在侧边栏
```

### Git 操作后，你将看到：

```
✓ 暂存文件: hello.py
✓ 创建提交: "Add hello.py with Hello World message"
✓ 提交哈希: abc123d
```

---

## 常见问题

### ❌ 无法连接到 OpenCode

**原因**：OpenCode 服务器未运行

**解决方案**：

```bash
# 检查 OpenCode 是否在运行
lsof -i :4096

# 如果没有输出，启动 OpenCode
cd your-project
opencode
```

### ❌ 端口已被占用

**原因**：默认端口被其他程序占用

**解决方案**：

```bash
# 使用自定义端口
openchamber --port 3002

# 或设置环境变量
OPENCHAMBER_PORT=3002 openchamber --daemon
```

### ❌ 文件未创建成功

**原因**：AI 没有写入权限或目标目录不存在

**解决方案**：

1. 确保当前工作目录是项目目录
2. 检查目录写入权限：`ls -la`
3. 在聊天中确认 AI 显示的错误信息

### ❌ Git 操作失败

**原因**：Git 仓库未初始化或配置不正确

**解决方案**：

```bash
# 初始化 Git 仓库
git init

# 配置用户信息
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

---

## 下一步学习

恭喜！你已经完成了 OpenChamber 的快速入门 🎉

继续探索以下主题：

### 核心概念

- 📖 [项目概述](./01-project-overview.md) - 了解 OpenChamber 的定位和功能
- 🏗️ [架构设计](./03-architecture.md) - 深入理解系统架构
- 📁 [代码结构](./04-code-structure.md) - 熟悉代码组织方式

### 开发指南

- 🛠️ [开发环境搭建](./05-development-setup.md) - 设置本地开发环境
- 🤝 [贡献指南](./07-contributing.md) - 参与项目贡献

### 高级功能

- **时间线分支** - 使用 `/undo` 和 `/redo` 探索不同方案
- **多代理运行** - 并行运行多个 AI 代理
- **GitHub 集成** - 直接从 Issue/PR 创建会话
- **远程访问** - 使用 Cloudflare Tunnel 实现远程访问

### 获取帮助

- 🐛 [GitHub Issues](https://github.com/btriapitsyn/openchamber/issues) - 报告问题
- 💬 [Discord 社区](https://discord.gg/ZYRSdnwwKA) - 加入讨论
- 📧 [邮件支持](mailto:support@opencode.ai) - 联系支持团队

---

## 快捷命令参考

| 命令 | 说明 |
|------|------|
| `/undo` | 撤销上一轮对话 |
| `/redo` | 重做已撤销的对话 |
| `/plan` | 进入计划模式 |
| `/build` | 进入构建模式 |
| `!command` | 执行 Shell 命令 |

---

**祝你使用愉快！Happy Coding! 🚀**

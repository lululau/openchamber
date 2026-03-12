# 开发环境搭建指南

本文档将指导你完成 OpenChamber 项目的开发环境搭建，包含所有必要的依赖安装、项目配置和开发服务器启动步骤。

## 目录

- [系统要求](#系统要求)
- [前置依赖安装](#前置依赖安装)
- [项目克隆与安装](#项目克隆与安装)
- [开发模式运行](#开发模式运行)
- [项目配置说明](#项目配置说明)
- [代码质量检查](#代码质量检查)
- [常见问题排查](#常见问题排查)

---

## 系统要求

### 基础要求

| 组件 | 最低版本 | 推荐版本 |
|------|---------|---------|
| Node.js | 20.0.0 | 20.x LTS |
| Bun | 1.3.5 | 1.3.5+ |
| Git | 2.x | 最新稳定版 |

### 操作系统支持

- **macOS**: 10.15+ (Catalina 或更高版本)
- **Linux**: 主流发行版 (Ubuntu 20.04+, Debian 11+, etc.)
- **Windows**: WSL2 环境 (原生支持开发中)

---

## 前置依赖安装

### 1. 安装 Node.js

#### macOS

```bash
# 使用 Homebrew
brew install node@20

# 或使用 nvm
nvm install 20
nvm use 20
```

#### Linux (Ubuntu/Debian)

```bash
# 使用 NodeSource 仓库
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# 验证安装
node --version
```

#### Windows (WSL2)

```bash
# 在 WSL2 中使用 NodeSource
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### 2. 安装 Bun

Bun 是该项目使用的包管理器和运行时。

```bash
# 使用官方安装脚本
curl -fsSL https://bun.sh/install | bash

# 验证安装
bun --version
```

### 3. 桌面应用开发依赖 (可选)

如果需要开发 macOS 桌面应用，需要安装以下依赖：

#### Rust 工具链

```bash
# 使用 rustup 安装
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# 验证安装
rustc --version
cargo --version
```

#### Tauri CLI

```bash
# 安装 Tauri CLI
cargo install tauri-cli@^2

# 或使用项目中的本地安装（推荐）
# 无需单独安装，会自动使用项目中的版本
```

#### Xcode Command Line Tools (macOS)

```bash
xcode-select --install
```

### 4. VS Code 扩展开发依赖 (可选)

如果需要开发 VS Code 扩展：

```bash
# 安装 vsce (VS Code Extension Manager)
bun add -g @vscode/vsce
```

---

## 项目克隆与安装

### 1. 克隆仓库

```bash
# HTTPS 方式
git clone https://github.com/btriapitsyn/openchamber.git
cd openchamber

# 或使用 SSH 方式（推荐）
git clone git@github.com:btriapitsyn/openchamber.git
cd openchamber
```

### 2. 安装依赖

使用 Bun 安装所有依赖（包括工作区中的所有包）：

```bash
bun install
```

这将自动：
- 安装根目录依赖
- 安装 `packages/ui` 的依赖
- 安装 `packages/web` 的依赖
- 安装 `packages/desktop` 的依赖
- 安装 `packages/vscode` 的依赖
- 应用 patch-package 补丁

**预计时间**: 首次安装约 2-5 分钟，取决于网络速度。

---

## 开发模式运行

OpenChamber 是一个 Monorepo 项目，包含多个运行时。你可以根据开发需求选择运行不同的模式。

### 项目结构概览

```
packages/
├── ui/        # 共享的 React 组件、hooks、stores 和主题系统
├── web/       # Web 服务器 (Express) + 前端 (Vite) + CLI
├── desktop/   # Tauri macOS 桌面应用（Web UI 的轻量包装）
└── vscode/    # VS Code 扩展（扩展主机 + webview）
```

### Web 开发

Web 开发有两种模式可供选择：

#### 模式 1: 完整构建 (无需 HMR)

```bash
bun run dev:web:full
```

- **端口**: 3001 (服务器 + 静态文件)
- **特点**: 完整的生产构建，无热更新
- **使用场景**: 测试生产构建、最终验证
- **注意**: 代码修改后需手动刷新浏览器

#### 模式 2: 开发模式 (带 HMR，推荐)

```bash
bun run dev:web:hmr
```

- **Vite HMR 端口**: 5180 (默认)
- **API 端口**: 3902 (默认)
- **特点**: 支持热模块替换 (HMR)，代码修改即时生效
- **重要**: 请打开 Vite URL (http://localhost:5180) 而非后端 URL
- **使用场景**: 日常开发

#### 环境变量配置

```bash
# 自定义端口
OPENCHAMBER_PORT=3001 bun run dev:web:full

# HMR 模式自定义端口
OPENCHAMBER_HMR_UI_PORT=5180 OPENCHAMBER_HMR_API_PORT=3902 bun run dev:web:hmr
```

### 桌面应用开发 (macOS)

```bash
bun run desktop:dev
```

这将启动 Tauri 开发模式，包含：
- WebView DevTools 支持
- 独立的开发图标
- 热重载支持

**首次运行**: 会自动下载 Tauri 依赖，可能需要 5-10 分钟。

### VS Code 扩展开发

#### 监听模式（推荐）

```bash
bun run vscode:dev
```

这将：
- 监听扩展代码变化
- 监听 webview 代码变化
- 自动重新构建

#### 在 VS Code 中测试

```bash
# 构建扩展
bun run vscode:build

# 在 VS Code 中加载扩展
code --extensionDevelopmentPath="$(pwd)/packages/vscode"
```

这将打开一个新的 VS Code 窗口，其中已加载你的扩展。

### 共享 UI 开发

`packages/ui` 是一个源码级库，没有独立的开发服务器。

在开发过程中，`bun run dev` 会自动对其运行类型检查的监听模式。

如果只想运行 UI 的类型检查：

```bash
bun run dev:ui
```

---

## 项目配置说明

### TypeScript 配置

项目使用 TypeScript 严格模式。所有包都有自己的 `tsconfig.json`：

```bash
# 检查所有包的类型
bun run type-check

# 检查特定包
bun run type-check:web
bun run type-check:ui
bun run type-check:desktop
```

### Lint 配置

项目使用 ESLint 进行代码检查：

```bash
# 检查所有包
bun run lint

# 检查特定包
bun run lint:web
bun run lint:ui
```

### 构建配置

```bash
# 构建所有包
bun run build

# 构建特定包
bun run build:web
bun run build:ui
bun run build:desktop
```

### 清理构建产物

```bash
# 清理所有包的构建产物
bun run clean
```

---

## 代码质量检查

在提交代码前，请确保通过所有检查：

### 1. 类型检查

```bash
bun run type-check
```

**必须通过** - TypeScript 严格模式下无类型错误。

### 2. Lint 检查

```bash
bun run lint
```

**必须通过** - 无 ESLint 错误或警告。

### 3. 构建测试

```bash
bun run build
```

**必须成功** - 所有包能够成功构建。

### 提交前检查脚本

```bash
# 完整的发布前准备流程
bun run release:prepare
```

这将依次运行：构建 → 类型检查 → Lint

---

## 常见问题排查

### 依赖安装问题

#### 问题: `bun install` 失败

**解决方案**:

```bash
# 清理缓存并重新安装
rm -rf node_modules packages/*/node_modules
bun install
```

#### 问题: 某些原生模块编译失败

**解决方案**:

```bash
# 确保安装了构建工具
# macOS
xcode-select --install

# Ubuntu/Debian
sudo apt-get install build-essential
```

### 开发服务器问题

#### 问题: 端口已被占用

**解决方案**:

```bash
# 查找占用端口的进程
lsof -i :3001
lsof -i :5180

# 终止进程
kill -9 <PID>

# 或使用其他端口
OPENCHAMBER_PORT=3002 bun run dev:web:full
```

#### 问题: HMR 不工作

**解决方案**:

1. 确保你打开的是 Vite URL (`http://localhost:5180`) 而非后端 URL
2. 检查防火墙设置
3. 清理浏览器缓存

### 桌面应用问题

#### 问题: Tauri 构建失败

**解决方案**:

```bash
# 确保安装了 Rust 和 Xcode Command Line Tools
rustc --version
xcode-select --install

# 清理并重新构建
cd packages/desktop
rm -rf src-tauri/target
cd ../..
bun run desktop:build
```

#### 问题: 桌面应用无法启动开发模式

**解决方案**:

```bash
# 确保 sidecar 已构建
bun run --cwd packages/desktop build:sidecar

# 然后启动开发模式
bun run desktop:dev
```

### VS Code 扩展问题

#### 问题: 扩展无法加载

**解决方案**:

```bash
# 确保扩展已构建
bun run vscode:build

# 检查 VS Code 版本 (需要 >= 1.85.0)
code --version

# 重新加载扩展
code --extensionDevelopmentPath="$(pwd)/packages/vscode"
```

### 类型检查问题

#### 问题: TypeScript 类型错误

**解决方案**:

```bash
# 清理构建缓存
rm -rf packages/*/dist

# 重新运行类型检查
bun run type-check

# 如果是 workspace 包的类型问题，尝试重新安装依赖
bun install
```

### 性能问题

#### 问题: 开发模式响应缓慢

**解决方案**:

1. 关闭不必要的浏览器扩展
2. 使用 `dev:web:full` 模式（无 HMR 开销）
3. 增加系统内存或关闭其他应用

---

## 下一步

环境搭建完成后，建议继续阅读：

- [项目概述](./01-project-overview.md) - 了解项目整体情况
- [技术栈](./02-tech-stack.md) - 了解项目使用的技术
- [架构设计](./03-architecture.md) - 深入理解系统架构
- [代码结构](./04-code-structure.md) - 熟悉代码组织方式

## 获取帮助

如遇到无法解决的问题：

- 在 GitHub 上提交 [Issue](https://github.com/btriapitsyn/openchamber/issues)
- 加入 [Discord 社区](https://discord.gg/ZYRSdnwwKA) 获取帮助
- 查看 [CONTRIBUTING.md](../CONTRIBUTING.md) 了解贡献指南

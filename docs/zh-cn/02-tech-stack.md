# 技术栈

本文档详细说明了 OpenChamber 项目所使用的技术栈及其版本要求。

## 目录

- [运行时环境](#运行时环境)
- [前端技术](#前端技术)
- [后端技术](#后端技术)
- [AI/LLM 相关](#aillm-相关)
- [桌面应用](#桌面应用)
- [VS Code 扩展](#vs-code-扩展)
- [开发工具](#开发工具)
- [CI/CD](#cicd)

---

## 运行时环境

| 技术 | 版本要求 | 说明 |
|------|----------|------|
| **Bun** | `^1.3.5` | 主要的 JavaScript 运行时和包管理器 |
| **Node.js** | `>=20.0.0` | 引擎最低版本要求（某些场景下需要） |
| **Rust** | stable (2021 edition) | 桌面应用原生层需要 |

---

## 前端技术

### 核心框架

| 技术 | 版本 | 用途 |
|------|------|------|
| **React** | `^19.1.1` | UI 框架 |
| **TypeScript** | `~5.8.3` | 类型安全的 JavaScript 超集 |
| **Vite** | `^7.1.2` | 前端构建工具 |

### UI 组件与样式

| 技术 | 版本 | 用途 |
|------|------|------|
| **TailwindCSS** | `^4.0.0` | 实用优先的 CSS 框架 |
| **PostCSS** | `^4.0.0` | CSS 转换工具 |
| **Autoprefixer** | `^10.4.21` | CSS 自动添加浏览器前缀 |
| **Radix UI** | 多个包 | 无样式的可访问组件库 |
| **HeroUI** | `^2.4.23` | UI 组件系统 |
| **next-themes** | `^0.4.6` | 主题切换功能 |
| **@remixicon/react** | `^4.7.0` | Remix 图标库 |
| **Motion** | `^12.23.24` | 动画库 |

### 编辑器组件

| 技术 | 版本 | 用途 |
|------|------|------|
| **CodeMirror 6** | 多个包 | 代码编辑器核心 |
| **@codemirror/lang-*** | 多个包 | 各种语言支持 (JS, TS, Python, Rust, Go, C++, SQL, YAML, JSON, Markdown, CSS, HTML 等) |
| **@lezer/highlight** | `^1.2.3` | 语法高亮 |

### 状态管理与数据处理

| 技术 | 版本 | 用途 |
|------|------|------|
| **Zustand** | `^5.0.8` | 轻量级状态管理 |
| **Zod** | `^4.3.6` | TypeScript 优先的模式验证 |
| **simple-git** | `^3.28.0` | Git 操作封装 |
| **yaml** | `^2.8.1` | YAML 解析 |

### Markdown 与代码高亮

| 技术 | 版本 | 用途 |
|------|------|------|
| **react-markdown** | `^10.1.0` | Markdown 渲染 |
| **remark-gfm** | `^4.0.1` | GitHub 风格 Markdown |
| **react-syntax-highlighter** | `^15.6.6` | 语法高亮 |
| **Prism.js** | `^1.30.0` | 代码高亮库 |
| **beautiful-mermaid** | `^1.1.3` | Mermaid 图表渲染 |

### 虚拟化与交互

| 技术 | 版本 | 用途 |
|------|------|------|
| **@tanstack/react-virtual** | `^3.13.18` | 虚拟滚动 |
| **@dnd-kit/*** | 多个包 | 拖拽功能 |
| **cmdk** | `^1.1.1` | 命令面板组件 |
| **sonner** | `^2.0.7` | Toast 通知 |

### 终端与工具

| 技术 | 版本 | 用途 |
|------|------|------|
| **Ghostty Web** | `^0.4.0` | Web 终端 |
| **node-pty** | `^1.1.0` | 伪终端支持 |
| **bun-pty** | `^0.4.5` | Bun 伪终端 |
| **qrcode** | `^1.5.4` | 二维码生成 |
| **html-to-image** | `^1.11.13` | HTML 转图片 |
| **heic2any** | `^0.0.4` | HEIC 图片转换 |

### 其他前端库

| 技术 | 版本 | 用途 |
|------|------|------|
| **fuse.js** | `^7.1.0` | 模糊搜索 |
| **streamdown** | `^2.2.0` | 流式 Markdown |
| **@pierre/diffs** | `1.1.0-beta.13` | 差异视图 |
| **@streamdown/code** | `^1.0.2` | 流式代码 |
| **jose** | `^6.1.3` | JWT 处理 |
| **web-push** | `^3.6.7` | Web 推送通知 |
| **@octokit/rest** | `^22.0.1` | GitHub API 客户端 |

---

## 后端技术

### Web 服务器

| 技术 | 版本 | 用途 |
|------|------|------|
| **Express** | `^5.1.0` | Web 框架 |
| **ws** | `^8.18.3` | WebSocket 服务器 |
| **http-proxy-middleware** | `^3.0.5` | HTTP 代理中间件 |

### 工具库

| 技术 | 版本 | 用途 |
|------|------|------|
| **adm-zip** | `^0.5.16` | ZIP 文件处理 |
| **strip-json-comments** | `^5.0.3` | 移除 JSON 注释 |
| **jsonc-parser** | `^3.3.1` | JSONC 解析 |
| **qrcode-terminal** | `^0.12.0` | 终端二维码显示 |
| **cors** | `^2.8.5` | CORS 支持 |

---

## AI/LLM 相关

| 技术 | 版本 | 用途 |
|------|------|------|
| **@opencode-ai/sdk** | `^1.2.20` | OpenCode AI SDK 核心库 |
| **OpenAI** | `^4.79.0` | OpenAI API 客户端（可选） |

---

## 桌面应用

### Tauri

| 技术 | 版本 | 用途 |
|------|------|------|
| **Tauri** | `^2.9.4` | 跨平台桌面应用框架 |
| **tauri-plugin-dialog** | `^2.4.2` | 对话框插件 |
| **tauri-plugin-log** | `^2.7.1` | 日志插件 |
| **tauri-plugin-shell** | `^2.3.3` | Shell 命令插件 |
| **tauri-plugin-notification** | `^2.3.3` | 通知插件 |
| **tauri-plugin-updater** | `^2.0.0` | 自动更新插件 |

### Rust 依赖

| 技术 | 版本 | 用途 |
|------|------|------|
| **tokio** | `^1.38` | 异步运行时 |
| **serde** | `^1.0.210` | 序列化/反序列化 |
| **reqwest** | `^0.12.4` | HTTP 客户端 |
| **anyhow** | `^1.0.86` | 错误处理 |

---

## VS Code 扩展

| 技术 | 版本 | 用途 |
|------|------|------|
| **VS Code Extension API** | `^1.85.0` | VS Code 扩展开发 |
| **@vscode/vsce** | `^3.2.1` | VS Code 扩展打包工具 |
| **esbuild** | `^0.24.2` | 扩展打包 |

---

## 开发工具

### 代码质量

| 技术 | 版本 | 用途 |
|------|------|------|
| **ESLint** | `^9.33.0` | JavaScript/TypeScript 代码检查 |
| **typescript-eslint** | `^8.39.1` | TypeScript ESLint 支持 |
| **eslint-plugin-react-hooks** | `^5.2.0` | React Hooks 规则 |
| **eslint-plugin-react-refresh** | `^0.4.20` | React Refresh 规则 |

### 开发辅助

| 技术 | 版本 | 用途 |
|------|------|------|
| **concurrently** | `^9.2.1` | 并发运行多个命令 |
| **nodemon** | `^3.1.7` | 自动重启开发服务器 |
| **tsx** | `^4.20.6` | TypeScript 执行器 |
| **patch-package** | `^8.0.0` | 包补丁管理 |
| **cross-env** | `^7.0.3` | 跨平台环境变量 |

### React Compiler

| 技术 | 版本 | 用途 |
|------|------|------|
| **babel-plugin-react-compiler** | `^1.0.0` | React 编译器优化 |

### 样式工具

| 技术 | 版本 | 用途 |
|------|------|------|
| **tw-animate-css** | `^1.3.8` | Tailwind CSS 动画 |
| **class-variance-authority** | `^0.7.1` | CVA 样式变体 |
| **clsx** | `^2.1.1` | 条件类名工具 |
| **tailwind-merge** | `^3.3.1` | Tailwind 类名合并 |

### 字体

| 技术 | 版本 | 用途 |
|------|------|------|
| **@fontsource/ibm-plex-sans** | `^5.1.1` | IBM Plex Sans 字体 |
| **@fontsource/ibm-plex-mono** | `^5.2.7` | IBM Plex Mono 字体 |
| **@ibm/plex** | `^6.4.1` | IBM Plex 字体集 |

---

## CI/CD

### GitHub Actions

项目使用 GitHub Actions 进行自动化构建和发布：

- `build-macos-arm64-dmg.yml` - macOS ARM64 DMG 构建
- `oc-integration.yml` - OpenCode 集成测试
- `oc-review.yml` - OpenCode 代码审查
- `release.yml` - 发布流程
- `vscode-extension.yml` - VS Code 扩展构建

---

## 系统要求

### macOS 开发环境

- macOS 14.0+ (桌面应用最低系统版本)
- Xcode Command Line Tools
- Rust stable toolchain (通过 rustup 安装)
- Tauri CLI (`cargo install tauri-cli@^2`)

### 跨平台开发

- 支持 macOS (Intel & ARM64)
- 支持 Web/PWA
- 支持 VS Code 扩展

---

## 架构概览

```
openchamber-monorepo/
├── packages/
│   ├── ui/          # 共享 React 组件库
│   ├── web/         # Web 服务器 + 前端 + CLI
│   ├── desktop/     # Tauri macOS 桌面应用
│   └── vscode/      # VS Code 扩展
└── .opencode/       # OpenCode CLI 依赖
```

---

## 快速开始

### 安装依赖

```bash
# 使用 Bun 安装所有依赖
bun install
```

### 开发模式

```bash
# 启动完整开发环境 (server + web + ui)
bun run dev

# 仅启动 Web 开发
bun run dev:web

# 启动桌面应用开发
bun run desktop:dev

# 启动 VS Code 扩展开发
bun run vscode:dev
```

### 构建

```bash
# 构建所有包
bun run build

# 仅构建 Web
bun run build:web

# 仅构建 UI
bun run build:ui

# 构建桌面应用
bun run desktop:build

# 构建 VS Code 扩展
bun run vscode:build
```

### 类型检查

```bash
# 检查所有包
bun run type-check

# 检查 Web
bun run type-check:web

# 检查 UI
bun run type-check:ui

# 检查桌面应用
bun run type-check:desktop
```

### 代码检查

```bash
# 检查所有包
bun run lint

# 检查 Web
bun run lint:web

# 检查 UI
bun run lint:ui

# 检查桌面应用
bun run lint:desktop
```

---

## 相关文档

- [项目概述](./01-project-overview.md)
- [AGENTS.md](../../AGENTS.md) - 技术架构参考
- [CONTRIBUTING.md](../../CONTRIBUTING.md) - 开发指南
- [CHANGELOG.md](../../CHANGELOG.md) - 版本历史

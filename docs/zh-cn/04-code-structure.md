# OpenChamber 代码结构

本文档详细说明 OpenChamber 项目的代码目录结构、关键文件位置和组织设计理念。

## 目录结构概览

```
openchamber/
├── packages/              # Monorepo 工作区
│   ├── ui/               # 共享 UI 组件库
│   ├── web/              # Web 应用 + 服务器 + CLI
│   ├── desktop/          # macOS 桌面应用 (Tauri)
│   └── vscode/           # VS Code 扩展
├── scripts/              # 构建和开发脚本
├── docs/                 # 项目文档
├── patches/              # npm 包补丁
└── tasks/                # 产品需求文档 (PRD)
```

## 核心源码目录详解

### `packages/ui/` - 共享 UI 层

**用途**：包含所有 React 组件、Hooks、状态管理和主题系统，被所有运行时共享使用。

```
packages/ui/src/
├── components/           # React 组件
│   ├── auth/            # 认证相关组件
│   ├── chat/            # 聊天界面 (33个子组件)
│   ├── comments/        # 代码评论组件
│   ├── desktop/         # 桌面特定组件
│   ├── layout/          # 布局组件
│   ├── sections/        # 设置页面区块
│   ├── session/         # 会话管理组件
│   ├── terminal/        # 终端组件
│   ├── ui/              # 通用 UI 原语 (51个组件)
│   ├── views/           # 完整页面视图
│   └── voice/           # 语音输入组件
├── hooks/               # 自定义 Hooks (42个)
├── stores/              # Zustand 状态管理 (40+ stores)
├── lib/                 # 工具函数和集成库 (60+ 文件)
├── contexts/            # React Context 提供者
├── styles/              # 全局样式和主题
├── types/               # TypeScript 类型定义
├── constants/           # 常量定义
└── assets/              # 静态资源
```

**关键文件**：

| 文件路径 | 用途 |
|---------|------|
| `src/main.tsx` | React 应用入口 |
| `src/App.tsx` | 主应用组件 |
| `src/stores/messageStore.ts` | 消息状态管理 (147KB，核心) |
| `src/stores/sessionStore.ts` | 会话状态管理 (82KB) |
| `src/stores/useConfigStore.ts` | 配置状态管理 (86KB) |
| `src/hooks/useEventStream.ts` | SSE 事件流处理 (95KB) |
| `src/lib/persistence.ts` | 数据持久化 (38KB) |
| `src/lib/openchamberConfig.ts` | OpenChamber 配置 (20KB) |
| `src/lib/gitApi.ts` | Git API 封装 (24KB) |

### `packages/web/` - Web 运行时

**用途**：Web 应用、Express 服务器和 CLI 工具的三位一体。

```
packages/web/
├── server/              # Express 服务器
│   ├── index.js        # 服务器入口 (454KB)
│   ├── lib/            # 服务器端库模块
│   │   ├── git/        # Git 操作模块
│   │   ├── github/     # GitHub 集成
│   │   ├── opencode/   # OpenCode 服务器集成
│   │   ├── quota/      # 配额管理
│   │   ├── terminal/   # 终端 WebSocket
│   │   ├── tts/        # 文字转语音
│   │   ├── skills-catalog/ # 技能目录
│   │   └── notifications/ # 通知系统
│   └── TERMINAL_INPUT_WS_PROTOCOL.md # 终端协议文档
├── src/                 # Web 应用入口
│   ├── main.tsx        # React 应用引导
│   ├── api/            # 客户端 API 调用
│   ├── sw.ts           # Service Worker (PWA)
│   └── pwa.d.ts        # PWA 类型定义
├── public/              # 静态资源
└── bin/                 # CLI 工具入口
```

**关键文件**：

| 文件路径 | 用途 |
|---------|------|
| `server/index.js` | Express 服务器主入口，包含所有 API 路由 |
| `server/lib/opencode/` | OpenCode 服务器集成核心 |
| `src/main.tsx` | Web 应用 React 入口 |
| `src/sw.ts` | PWA Service Worker |

### `packages/desktop/` - 桌面应用

**用途**：使用 Tauri v2 构建的 macOS 原生应用，作为 Web UI 的薄壳。

```
packages/desktop/
├── src-tauri/           # Tauri Rust 后端
│   ├── src/
│   │   ├── main.rs     # 主入口 (106KB，核心逻辑)
│   │   └── remote_ssh.rs # SSH 远程连接 (96KB)
│   ├── capabilities/   # Tauri 能力配置
│   ├── resources/      # 应用资源
│   ├── icons/          # 应用图标
│   └── sidecars/       # 侧车服务配置
├── public/              # 静态资源
├── scripts/             # 构建脚本
└── noop-dist/           # 空构建输出目录
```

**设计理念**：Desktop 应用只是一个原生壳，所有业务逻辑都在 `packages/web/server/` 中。Rust 代码仅处理：
- 原生菜单
- 文件对话框
- 系统通知
- 应用更新
- 深度链接
- SSH 远程连接

### `packages/vscode/` - VS Code 扩展

**用途**：在 VS Code 中集成 OpenChamber 的完整功能。

```
packages/vscode/
├── src/                 # 扩展主机进程
│   ├── extension.ts    # 扩展入口 (24KB)
│   ├── bridge.ts       # Webview 桥接 (135KB，核心)
│   ├── gitService.ts   # Git 服务 (84KB)
│   ├── opencodeConfig.ts # OpenCode 配置 (64KB)
│   ├── opencode.ts     # OpenCode 集成 (25KB)
│   ├── github*.ts      # GitHub 集成 (多个文件)
│   └── quotaProviders.ts # 配额提供者 (44KB)
├── webview/             # Webview 内容
│   ├── api/            # Webview API
│   └── (复用 packages/ui 的组件)
└── assets/              # 扩展资源
```

**关键文件**：

| 文件路径 | 用途 |
|---------|------|
| `src/extension.ts` | VS Code 扩展激活入口 |
| `src/bridge.ts` | 扩展与 Webview 通信桥 |
| `src/ChatViewProvider.ts` | 聊天视图提供者 |
| `src/SessionEditorPanelProvider.ts` | 会话编辑器面板 |

## 项目根目录结构

### 配置文件

| 文件 | 用途 |
|------|------|
| `package.json` | Monorepo 配置，依赖管理，构建脚本 |
| `bun.lock` | Bun 锁文件 (409KB) |
| `tsconfig.json` | TypeScript 配置 |
| `vite.config.ts` | Vite 构建配置 |
| `eslint.config.js` | ESLint 规则 |
| `components.json` | Shadcn/ui 组件配置 |
| `docker-compose.yml` | Docker 编排配置 |
| `Dockerfile` | Docker 镜像构建 |
| `Caddyfile` | Caddy 反向代理配置 |

### 文档文件

| 文件 | 用途 |
|------|------|
| `README.md` | 用户指南和快速开始 |
| `AGENTS.md` | AI 代理技术架构参考 |
| `CHANGELOG.md` | 版本更新历史 |
| `CONTRIBUTING.md` | 贡献指南 |
| `SECURITY.md` | 安全政策 |
| `LICENSE` | MIT 许可证 |

### 脚本目录

| 脚本 | 用途 |
|------|------|
| `scripts/bump-version.mjs` | 版本号升级 |
| `scripts/dev-web-*.mjs` | Web 开发服务器 |
| `scripts/install.sh` | 安装脚本 |
| `scripts/test-release-build.sh` | 发布构建测试 |
| `scripts/convert-vscode-theme.cjs` | VS Code 主题转换 |
| `scripts/generate-file-type-sprite.mjs` | 文件类型图标生成 |

## 代码组织设计理念

### 1. Monorepo 架构

使用单一仓库管理多个相关包，通过 `package.json` 的 `workspaces` 字段配置：

```json
{
  "workspaces": [
    "packages/*"
  ]
}
```

**优势**：
- 代码共享：`packages/ui` 被所有运行时共享
- 统一依赖：通过根目录 `bun.lock` 锁定版本
- 简化开发：一次安装，所有包就绪

### 2. 共享 UI，多运行时

```
                    ┌─────────────────┐
                    │  packages/ui    │
                    │  (React 组件)    │
                    └────────┬────────┘
                             │
        ┌────────────────────┼────────────────────┐
        │                    │                    │
        ▼                    ▼                    ▼
┌───────────────┐   ┌───────────────┐   ┌───────────────┐
│ packages/web  │   │packages/dsktp │   │packages/vscode│
│  (Vite + Web) │   │  (Tauri壳)    │   │ (Extension)   │
└───────────────┘   └───────────────┘   └───────────────┘
```

**关键决策**：
- 所有 React 组件在 `packages/ui` 中
- 运行时特定代码在各自包中
- 通过 TypeScript 路径别名导入：`@opencode-chamber/ui`

### 3. 状态管理层次

使用 Zustand 进行状态管理，按功能域划分：

| 状态域 | Store 文件 | 职责 |
|--------|-----------|------|
| 消息 | `messageStore.ts` | 消息 CRUD、分支、历史 |
| 会话 | `sessionStore.ts` | 会话列表、活动状态 |
| 配置 | `useConfigStore.ts` | 用户配置、项目配置 |
| Git | `useGitStore.ts` | Git 状态、操作 |
| GitHub | `useGitHubPrStatusStore.ts` | PR 状态同步 |
| 项目 | `useProjectsStore.ts` | 项目列表、元数据 |
| 终端 | `useTerminalStore.ts` | 终端会话、输出 |
| UI | `useUIStore.ts` | 界面状态、布局 |

### 4. 服务端模块化

服务器功能按领域划分独立模块，每个模块有自文档化：

```
packages/web/server/lib/
├── git/              # Git 操作
│   └── DOCUMENTATION.md
├── github/           # GitHub 集成
│   └── DOCUMENTATION.md
├── opencode/         # OpenCode 服务器
│   └── DOCUMENTATION.md
├── quota/            # 配额管理
│   └── DOCUMENTATION.md
├── terminal/         # 终端服务
│   └── DOCUMENTATION.md
└── ...
```

每个 `DOCUMENTATION.md` 包含：
- 模块职责
- 导出函数列表
- 使用示例
- 外部依赖

### 5. 类型安全

- 严格的 `tsconfig.json` 配置
- 所有组件使用 TypeScript
- 共享类型在 `packages/ui/src/types/` 中定义
- API 响应类型在 `packages/ui/src/lib/api/` 中定义

## 目录树可视化

以下是项目的完整目录树（省略 node_modules 和构建产物）：

```
openchamber/
├── .github/                # GitHub Actions 工作流
├── .opencode/              # OpenCode 本地数据
├── docs/                   # 项目文档
│   ├── references/         # 参考图片和资源
│   └── zh-cn/              # 中文文档
│       ├── 01-project-overview.md
│       ├── 02-tech-stack.md
│       ├── 03-architecture.md
│       └── 04-code-structure.md
├── packages/               # Monorepo 工作区
│   ├── ui/                 # 共享 UI 组件库
│   │   ├── src/
│   │   │   ├── components/ # 18个组件目录
│   │   │   ├── hooks/      # 42个自定义 Hooks
│   │   │   ├── stores/     # 40+ Zustand stores
│   │   │   ├── lib/        # 60+ 工具模块
│   │   │   ├── contexts/   # 11个 Context
│   │   │   ├── styles/     # 7个样式文件
│   │   │   ├── types/      # 17个类型文件
│   │   │   ├── constants/  # 3个常量文件
│   │   │   └── assets/     # 4个资源目录
│   │   ├── package.json
│   │   └── vite.config.ts
│   ├── web/                # Web 应用 + 服务器
│   │   ├── server/         # Express 服务器
│   │   │   ├── lib/        # 12个功能模块
│   │   │   └── index.js
│   │   ├── src/
│   │   │   ├── api/
│   │   │   ├── main.tsx
│   │   │   └── sw.ts
│   │   ├── public/
│   │   ├── bin/            # CLI 工具
│   │   └── package.json
│   ├── desktop/            # macOS 桌面应用
│   │   ├── src-tauri/      # Tauri Rust 后端
│   │   │   ├── src/
│   │   │   │   ├── main.rs
│   │   │   │   └── remote_ssh.rs
│   │   │   ├── capabilities/
│   │   │   ├── resources/
│   │   │   └── icons/
│   │   ├── public/
│   │   └── package.json
│   └── vscode/             # VS Code 扩展
│       ├── src/            # 扩展主机代码
│       │   ├── extension.ts
│       │   ├── bridge.ts
│       │   ├── gitService.ts
│       │   └── ... (21个文件)
│       ├── webview/        # Webview 内容
│       │   ├── api/
│       │   └── main.tsx
│       ├── assets/
│       └── package.json
├── scripts/                # 构建和开发脚本
│   ├── bump-version.mjs
│   ├── convert-vscode-theme.cjs
│   ├── dev-web-full.mjs
│   ├── dev-web-hmr.mjs
│   ├── docker-entrypoint.sh
│   ├── generate-file-type-sprite.mjs
│   ├── install.sh
│   └── test-release-build.sh
├── patches/                # npm 包补丁
│   └── ghostty-web+0.3.0.patch
├── tasks/                  # PRD 文档
│   ├── prd-openchamber.md
│   └── prd.json
├── .gitignore
├── .nvmrc
├── .env
├── .dockerignore
├── AGENTS.md               # AI 代理技术参考
├── bun.lock                # Bun 依赖锁
├── Caddyfile               # Caddy 配置
├── CHANGELOG.md            # 版本历史
├── components.json         # Shadcn/ui 配置
├── CONTRIBUTING.md         # 贡献指南
├── docker-compose.yml      # Docker 编排
├── Dockerfile              # Docker 镜像
├── eslint.config.js        # ESLint 规则
├── fix-deprecation.js      # 依赖修复脚本
├── LICENSE                 # MIT 许可证
├── package.json            # Monorepo 配置
├── postcss.config.js       # PostCSS 配置
├── README.md               # 用户指南
├── SECURITY.md             # 安全政策
├── tsconfig.json           # TypeScript 配置
├── vite-theme-plugin.ts    # Vite 主题插件
└── vite.config.ts          # Vite 构建配置
```

## 关键文件路径速查表

### 用户界面相关

| 功能 | 文件路径 |
|------|----------|
| 聊天界面 | `packages/ui/src/components/chat/` |
| 消息渲染 | `packages/ui/src/components/chat/message/` |
| 终端组件 | `packages/ui/src/components/terminal/` |
| 设置页面 | `packages/ui/src/components/views/SettingsView.tsx` |
| Git 侧边栏 | `packages/ui/src/components/sections/git/` |
| 主题系统 | `packages/ui/src/lib/theme/` |

### 状态管理相关

| 功能 | 文件路径 |
|------|----------|
| 消息状态 | `packages/ui/src/stores/messageStore.ts` |
| 会话状态 | `packages/ui/src/stores/sessionStore.ts` |
| 配置状态 | `packages/ui/src/stores/useConfigStore.ts` |
| Git 状态 | `packages/ui/src/stores/useGitStore.ts` |
| UI 状态 | `packages/ui/src/stores/useUIStore.ts` |

### 服务器相关

| 功能 | 文件路径 |
|------|----------|
| API 路由 | `packages/web/server/index.js` |
| OpenCode 集成 | `packages/web/server/lib/opencode/` |
| Git 操作 | `packages/web/server/lib/git/` |
| GitHub 集成 | `packages/web/server/lib/github/` |
| 终端 WebSocket | `packages/web/server/lib/terminal/` |

### 桌面应用相关

| 功能 | 文件路径 |
|------|----------|
| 主入口 | `packages/desktop/src-tauri/src/main.rs` |
| SSH 连接 | `packages/desktop/src-tauri/src/remote_ssh.rs` |
| Tauri 配置 | `packages/desktop/src-tauri/tauri.conf.json` |
| 应用图标 | `packages/desktop/src-tauri/icons/` |

### VS Code 扩展相关

| 功能 | 文件路径 |
|------|----------|
| 扩展入口 | `packages/vscode/src/extension.ts` |
| Webview 桥接 | `packages/vscode/src/bridge.ts` |
| Git 服务 | `packages/vscode/src/gitService.ts` |
| 聊天视图 | `packages/vscode/src/ChatViewProvider.ts` |

## 开发工作流建议

### 新增 UI 组件

1. 在 `packages/ui/src/components/` 相应目录下创建组件
2. 如需状态管理，在 `packages/ui/src/stores/` 创建 store
3. 在 `packages/ui/src/types/` 定义相关类型
4. 在相应运行时 (`web`/`desktop`/`vscode`) 中使用

### 新增 API 端点

1. 在 `packages/web/server/index.js` 添加路由
2. 将业务逻辑提取到 `packages/web/server/lib/` 新模块
3. 在 `packages/ui/src/lib/api/` 添加客户端调用函数
4. 在 `packages/web/src/api/` 添加类型定义

### 新增服务器模块

1. 在 `packages/web/server/lib/` 创建新目录
2. 实现 `DOCUMENTATION.md` 说明模块职责
3. 导出必要的函数和类型
4. 在 `packages/web/server/index.js` 中引入

## 相关文档

- [项目概览](./01-project-overview.md) - 了解项目整体定位
- [技术栈](./02-tech-stack.md) - 查看技术选型和版本
- [架构设计](./03-architecture.md) - 理解系统架构和数据流
- [AGENTS.md](../../AGENTS.md) - AI 代理技术架构参考

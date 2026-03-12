# 贡献指南

感谢您对 OpenChamber 项目的关注！我们欢迎任何形式的贡献，包括但不限于代码提交、Bug 报告、功能建议、文档改进等。

## 目录

- [开发环境准备](#开发环境准备)
- [代码风格规范](#代码风格规范)
- [Commit Message 规范](#commit-message-规范)
- [Pull Request 流程](#pull-request-流程)
- [测试要求](#测试要求)
- [Issue 报告](#issue-报告)
- [获取帮助](#获取帮助)

## 开发环境准备

### 前置要求

| 依赖项 | 最低版本 | 推荐版本 |
|--------|---------|---------|
| Node.js | 20.0.0 | 最新 LTS |
| Bun | 1.3.5+ | 最新稳定版 |
| Git | 2.x | 最新稳定版 |

### 桌面开发额外要求

- Rust 工具链
- Xcode Command Line Tools (macOS)
- Tauri CLI

### 克隆项目

```bash
git clone https://github.com/btriapitsyn/openchamber.git
cd openchamber
bun install
```

### 开发模式

#### Web 开发

| 脚本 | 说明 | 端口 |
|------|------|------|
| `bun run dev:web:full` | 构建监听 + Express 服务器。无 HMR — 更改后需手动刷新。 | `3001` (服务器 + 静态文件) |
| `bun run dev:web:hmr` | Vite 开发服务器 + Express API。**打开 Vite URL 以使用 HMR**，而非后端 URL。 | `5180` (Vite HMR), `3902` (API) |

两种模式均可通过环境变量自定义端口：
- `OPENCHAMBER_PORT`
- `OPENCHAMBER_HMR_UI_PORT`
- `OPENCHAMBER_HMR_API_PORT`

#### 桌面开发（Tauri）

```bash
bun run desktop:dev
```

以开发模式启动 Tauri，启用 WebView 开发者工具并使用独特的开发图标。

#### VS Code 扩展开发

```bash
bun run vscode:dev    # 监听模式（扩展 + webview 保存时重新构建）
```

在 VS Code 中测试：
```bash
bun run vscode:build && code --extensionDevelopmentPath="$(pwd)/packages/vscode"
```

#### 共享 UI (`packages/ui`)

无独立开发服务器 — 这是一个被其他包消费的源代码级库。开发期间，`bun run dev` 会以监听模式运行类型检查。

## 代码风格规范

### React 组件规范

- **仅使用函数式组件**：不使用类组件
- **TypeScript 严格模式**：禁止使用 `any` 类型，除非有充分理由
- **主题一致性**：使用 `packages/ui/src/lib/theme/` 中现有的主题颜色/排版，不要添加新的
- **深色/浅色主题支持**：所有组件必须同时支持浅色和深色主题
- **代码可读性**：优先使用提前返回和 `if/else`/`switch`，而非嵌套三元表达式

### 样式规范

- **Tailwind CSS v4**：所有样式使用 Tailwind v4
- **排版**：通过 `packages/ui/src/lib/typography.ts` 使用预定义的排版样式
- **UI 原语**：优先使用项目中的 UI 组件库（Radix UI、HeroUI）

### TypeScript 规范

项目使用 TypeScript 严格模式，配置位于 `packages/*/tsconfig.json`：

```json
{
  "compilerOptions": {
    "strict": true,
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "bundler"
  }
}
```

### ESLint 规范

项目使用 ESLint 进行代码检查，配置位于 `eslint.config.js`：

- TypeScript ESLint 规则
- React Hooks 规则
- React Refresh 规则（Vite）

运行 Lint 检查：
```bash
bun run lint         # 检查所有包
bun run lint:ui      # 仅检查 UI 包
bun run lint:web     # 仅检查 Web 包
bun run lint:desktop # 仅检查 Desktop 包
```

### 代码格式建议

```typescript
// ✅ 推荐：使用提前返回
function Component({ prop }) {
  if (!prop) return null

  return <div>{prop}</div>
}

// ❌ 避免：嵌套三元表达式
function Component({ prop }) {
  return prop ? <div>{prop}</div> : prop === null ? <span>Loading</span> : null
}

// ✅ 推荐：解构 props
function Component({ title, description, onAction }) {
  // ...
}

// ✅ 推荐：使用现有的主题变量
import { colors } from '@/lib/theme'
const style = { color: colors.foreground.primary }
```

## Commit Message 规范

### 格式

遵循 [Conventional Commits](https://www.conventionalcommits.org/) 规范：

```
<type>[: <scope>]: <description>

[optional body]

[optional footer]
```

### 类型（Type）

| 类型 | 说明 | 示例 |
|------|------|------|
| `feat` | 新功能 | `feat: add spell check toggle for desktop` |
| `fix` | Bug 修复 | `fix: allow modified Enter send in narrow layouts` |
| `refactor` | 代码重构 | `refactor: modularize session sidebar` |
| `perf` | 性能优化 | `perf: speed up desktop startup` |
| `docs` | 文档更新 | `docs: rewrite READMEs` |
| `chore` | 构建/工具更新 | `chore: update readme` |
| `style` | 代码格式调整 | `style: standardize bullet point formatting` |

### 作用域（Scope）

可选，用于指定提交影响的范围：

- `cli` - CLI 相关
- `desktop` - 桌面应用
- `vscode` - VS Code 扩展
- `web` - Web 应用
- `ui` - 共享 UI 组件

### 示例

```bash
# 基本格式
git commit -m "feat: add optional activity header timestamps in chat settings"

# 带作用域
git commit -m "fix(cli): surface tunnel bootstrap connect URL for --try-cf-tunnel"

# 关联 Issue
git commit -m "feat: prioritize worktrees with active sessions in sidebar (#638)"
```

## Pull Request 流程

### 1. Fork 和创建分支

```bash
# Fork 项目到你的 GitHub 账户
# 克隆你的 fork
git clone https://github.com/YOUR_USERNAME/openchamber.git
cd openchamber

# 添加上游仓库
git remote add upstream https://github.com/btriapitsyn/openchamber.git

# 创建特性分支
git checkout -b feature/your-feature-name
# 或
git checkout -b fix/your-bug-fix
```

### 2. 进行更改

- 遵循[代码风格规范](#代码风格规范)
- 编写清晰的代码和注释
- 保持更改简洁且专注

### 3. 运行质量检查

在提交 PR 之前，请确保以下检查全部通过：

```bash
# 类型检查（必须通过）
bun run type-check

# Lint 检查（必须通过）
bun run lint

# 构建检查（必须成功）
bun run build
```

### 4. 提交更改

```bash
git add .
git commit -m "feat: 简洁描述你的更改"
```

### 5. 推送并创建 PR

```bash
git push origin feature/your-feature-name
```

然后在 GitHub 上创建 Pull Request。

### PR 描述模板

```markdown
## 更改内容
简要描述你的更改内容。

## 更改原因
解释为什么需要这个更改。

## 测试
描述你如何测试这些更改。

## 截图（如适用）
添加相关截图帮助审核者理解更改。
```

### PR 审核要点

- [ ] 代码通过所有质量检查（type-check、lint、build）
- [ ] 遵循项目的代码风格规范
- [ ] 添加了必要的注释或文档
- [ ] 更新了相关的文档（如需要）
- [ ] PR 描述清晰，说明了更改的内容和原因

## 测试要求

### 当前测试状态

项目目前**未**设置自动化测试框架，主要依赖以下质量保证措施：

### 质量检查

在提交代码前，请确保以下检查通过：

| 命令 | 说明 | 状态要求 |
|------|------|---------|
| `bun run type-check` | TypeScript 类型检查 | ✅ 必须通过 |
| `bun run lint` | ESLint 代码规范检查 | ✅ 必须通过 |
| `bun run build` | 构建所有包 | ✅ 必须成功 |

### 手动测试建议

根据你更改的内容，进行相应的手动测试：

#### UI 更改
- [ ] 在浅色和深色主题下测试
- [ ] 测试不同屏幕尺寸下的响应式布局
- [ ] 测试键盘导航和可访问性

#### 功能更改
- [ ] 测试所有相关的用户流程
- [ ] 验证错误处理和边界情况
- [ ] 确保与现有功能兼容

#### API 更改
- [ ] 验证请求和响应格式
- [ ] 测试错误处理
- [ ] 确保向后兼容（如适用）

### 特定运行时测试

- **Desktop**: 在 macOS 上测试桌面应用
- **Web**: 在主流浏览器中测试
- **VS Code**: 在 VS Code 中测试扩展功能

## Issue 报告

### Bug 报告

报告 Bug 时，请提供以下信息：

- **问题描述**：详细描述发生了什么以及你期望发生什么
- **复现步骤**：如何复现这个问题
- **运行环境**：问题发生的运行环境
  - Desktop (macOS)
  - Desktop Web
  - Mobile (Web/PWA)
  - VS Code 扩展
- **版本信息**：已知的版本号（如 1.2.3）
- **截图/录屏**：有助于理解问题的视觉材料（可选）
- **相关日志**：相关的错误日志（可选）

### 功能请求

提交功能请求时，请说明：

- **功能描述**：你希望添加或更改什么功能
- **使用场景**：你试图完成什么任务，希望如何实现
- **额外信息**：链接、截图、原型图、限制条件等（可选）

### Issue 模板

项目提供了 GitHub Issue 模板，可以在 [GitHub Issues](https://github.com/btriapitsyn/openchamber/issues) 页面使用预设的模板。

## 非开发者贡献

即使你不是开发者，也可以帮助项目：

- **报告 Bug 或 UX 问题**：即使是"这让人困惑"这样的反馈也很有价值
- **跨平台测试**：在不同设备、浏览器或操作系统版本上测试
- **功能建议**：通过 Issue 提出功能或改进建议
- **社区支持**：在 Discord 中帮助其他用户

## 获取帮助

如果你有任何问题：

- 📝 在 [GitHub Issues](https://github.com/btriapitsyn/openchamber/issues) 提问
- 💬 加入 [Discord](https://discord.gg/ZYWSdnwwKA) 社区讨论
- 📖 查看 [AGENTS.md](../../AGENTS.md) 了解详细的技术架构

## 相关文档

- [项目概述](./01-project-overview.md)
- [技术栈](./02-tech-stack.md)
- [架构设计](./03-architecture.md)
- [代码结构](./04-code-structure.md)
- [开发环境配置](./05-development-setup.md)
- [API 参考](./06-api-reference.md)

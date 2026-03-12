# API 参考文档

本文档详细介绍了 OpenChamber 的 HTTP API、WebSocket API 和事件流接口，包括请求/响应格式、数据类型定义和使用示例。

## 目录

- [概述](#概述)
- [认证](#认证)
- [事件 API (Server-Sent Events)](#事件-api-server-sent-events)
- [系统 API](#系统-api)
- [配置 API](#配置-api)
- [GitHub API](#github-api)
- [Git API](#git-api)
- [文件系统 API](#文件系统-api)
- [终端 API](#终端-api)
- [会话 API](#会话-api)
- [推送通知 API](#推送通知-api)
- [语音与 TTS API](#语音与-tts-api)
- [配额 API](#配额-api)
- [数据类型定义](#数据类型定义)

## 概述

OpenChamber 提供 RESTful HTTP API 和 WebSocket API，用于与后端服务通信。所有 API 端点以 `/api` 为前缀（除健康检查和认证相关端点）。

### 基础 URL

- 本地开发: `http://localhost:3001`
- 桌面应用: 通过本地服务器访问
- 远程访问: 通过 Cloudflare Tunnel 提供的 URL

### 通用响应格式

成功响应通常返回 JSON 格式：

```json
{
  "data": { ... }
}
```

错误响应格式：

```json
{
  "error": "错误消息描述"
}
```

### 状态码

- `200` - 成功
- `201` - 创建成功
- `400` - 请求参数错误
- `401` - 未授权
- `403` - 禁止访问
- `404` - 资源不存在
- `500` - 服务器内部错误

---

## 认证

OpenChamber 提供可选的基于密码的认证机制，用于保护 Web 界面访问。

### 获取认证状态

```http
GET /auth/session
```

**响应示例：**

```json
{
  "authenticated": true,
  "username": "user"
}
```

### 创建会话（登录）

```http
POST /auth/session
Content-Type: application/json

{
  "password": "your-password"
}
```

**响应示例：**

```json
{
  "success": true,
  "token": "session-token-string"
}
```

### 连接检查

```http
GET /connect
```

用于验证 OpenChamber 后端连接状态。

---

## 事件 API (Server-Sent Events)

OpenChamber 使用 Server-Sent Events (SSE) 向客户端推送实时事件流。

### 会话事件流

```http
GET /api/event?sessionId={sessionId}
```

**参数：**
- `sessionId` (必需) - 会话 ID

**响应类型：** `text/event-stream`

**事件类型：**
- `session.message` - 新消息
- `session.status` - 会话状态变更
- `session.agent_thought` - 代理思考过程
- `session.error` - 会话错误

### 全局事件流

```http
GET /api/global/event
```

获取全局系统事件，不限于特定会话。

**事件类型：**
- `quota.warning` - 配额警告
- `system.notification` - 系统通知
- `project.update` - 项目更新

---

## 系统 API

### 健康检查

```http
GET /health
```

**响应示例：**

```json
{
  "status": "ok",
  "uptime": 1234567890
}
```

### 系统信息

```http
GET /api/system/info
```

**响应示例：**

```json
{
  "version": "1.8.5",
  "platform": "darwin",
  "nodeVersion": "v20.10.0",
  "bunVersion": "1.3.5"
}
```

### 关闭服务器

```http
POST /api/system/shutdown
```

**请求体：** (可选)

```json
{
  "restart": false
}
```

### 更新检查

```http
GET /api/openchamber/update-check
```

检查是否有新版本可用。

**响应示例：**

```json
{
  "hasUpdate": true,
  "latestVersion": "1.9.0",
  "currentVersion": "1.8.5"
}
```

### 安装更新

```http
POST /api/openchamber/update-install
```

启动更新安装流程。

### Cloudflare Tunnel API

#### 检查 Tunnel 状态

```http
GET /api/openchamber/tunnel/check
GET /api/openchamber/tunnel/status
```

**响应示例：**

```json
{
  "running": true,
  "url": "https://abc.trycloudflare.com",
  "mode": "quick"
}
```

#### 启动 Tunnel

```http
POST /api/openchamber/tunnel/start
```

**请求体：**

```json
{
  "mode": "quick",
  "namedToken": "optional-named-token"
}
```

#### 停止 Tunnel

```http
POST /api/openchamber/tunnel/stop
```

#### 设置命名令牌

```http
PUT /api/openchamber/tunnel/named-tunnel
```

**请求体：**

```json
{
  "token": "your-named-tunnel-token"
}
```

---

## 配置 API

配置 API 用于管理 OpenCode 的各种配置，包括设置、Agent、MCP、命令和技能。

### 获取设置

```http
GET /api/config/settings
```

**响应示例：**

```json
{
  "settings": {
    "theme": "github-dark",
    "fontSize": 14,
    "fontFamily": "system-ui",
    "model": "claude-opus-4"
  }
}
```

### 更新设置

```http
PUT /api/config/settings
Content-Type: application/json

{
  "theme": "github-dark",
  "fontSize": 14
}
```

### 获取主题列表

```http
GET /api/config/themes
```

**响应示例：**

```json
{
  "themes": [
    { "id": "github-dark", "name": "GitHub Dark", "variant": "dark" },
    { "id": "github-light", "name": "GitHub Light", "variant": "light" }
  ]
}
```

### Agent 管理

#### 获取 Agent 列表

```http
GET /api/config/agents
```

#### 获取特定 Agent

```http
GET /api/config/agents/:name
```

#### 获取 Agent 配置

```http
GET /api/config/agents/:name/config
```

#### 创建 Agent

```http
POST /api/config/agents/:name
Content-Type: application/json

{
  "description": "Agent 描述",
  "model": "claude-opus-4",
  "permissions": ["read", "write"]
}
```

#### 更新 Agent

```http
PATCH /api/config/agents/:name
Content-Type: application/json

{
  "description": "更新的描述"
}
```

#### 删除 Agent

```http
DELETE /api/config/agents/:name
```

### MCP（Model Context Protocol）管理

#### 获取 MCP 列表

```http
GET /api/config/mcp
```

#### 获取特定 MCP

```http
GET /api/config/mcp/:name
```

#### 创建 MCP

```http
POST /api/config/mcp/:name
Content-Type: application/json

{
  "type": "stdio",
  "command": "node",
  "args": ["path/to/mcp-server"]
}
```

#### 更新 MCP

```http
PATCH /api/config/mcp/:name
```

#### 删除 MCP

```http
DELETE /api/config/mcp/:name
```

### 命令管理

#### 获取命令列表

```http
GET /api/config/commands
```

#### 获取特定命令

```http
GET /api/config/commands/:name
```

**响应示例：**

```json
{
  "name": "commit",
  "description": "Create git commit",
  "template": "commit"
}
```

#### 创建命令

```http
POST /api/config/commands/:name
Content-Type: application/json

{
  "description": "命令描述",
  "template": "命令模板"
}
```

#### 更新命令

```http
PATCH /api/config/commands/:name
```

#### 删除命令

```http
DELETE /api/config/commands/:name
```

### 技能（Skills）管理

#### 获取技能列表

```http
GET /api/config/skills
```

**响应示例：**

```json
{
  "skills": [
    {
      "name": "code-review",
      "scope": "user",
      "description": "Code review skill"
    }
  ]
}
```

#### 获取技能目录

```http
GET /api/config/skills/catalog
```

**查询参数：**
- `source` - 可选，指定来源

#### 获取技能目录来源

```http
GET /api/config/skills/catalog/source
```

**响应示例：**

```json
{
  "sources": [
    {
      "id": "anthropic",
      "name": "Anthropic Official",
      "url": "github.com/anthropics/claude-code"
    }
  ]
}
```

#### 扫描技能

```http
POST /api/config/skills/scan
Content-Type: application/json

{
  "source": "github.com/owner/repo",
  "subpath": "/skills"
}
```

#### 安装技能

```http
POST /api/config/skills/install
Content-Type: application/json

{
  "selections": [
    {
      "repoSource": "github.com/owner/repo",
      "skillName": "skill-name",
      "scope": "user"
    }
  ],
  "conflictPolicy": "prompt"
}
```

#### 获取特定技能

```http
GET /api/config/skills/:name
```

#### 获取技能文件

```http
GET /api/config/skills/:name/files/*filePath
```

#### 创建技能

```http
POST /api/config/skills/:name
Content-Type: application/json

{
  "description": "技能描述",
  "scope": "user"
}
```

#### 更新技能

```http
PATCH /api/config/skills/:name
```

#### 写入技能文件

```http
PUT /api/config/skills/:name/files/*filePath
Content-Type: text/markdown

技能文件内容...
```

#### 删除技能文件

```http
DELETE /api/config/skills/:name/files/*filePath
```

#### 删除技能

```http
DELETE /api/config/skills/:name
```

### 重新加载配置

```http
POST /api/config/reload
```

重新加载所有配置文件，包括技能、命令和 Agent。

---

## GitHub API

GitHub API 提供 GitHub OAuth 认证、PR 管理、Issue 查询等功能。

### 认证

#### 获取认证状态

```http
GET /api/github/auth/status
```

**响应示例：**

```json
{
  "authenticated": true,
  "user": {
    "login": "username",
    "avatarUrl": "https://avatars.githubusercontent.com/u/123456"
  },
  "accounts": [
    {
      "id": "account-1",
      "login": "username",
      "avatarUrl": "https://avatars.githubusercontent.com/u/123456"
    }
  ]
}
```

#### 启动设备流程认证

```http
POST /api/github/auth/start
```

**响应示例：**

```json
{
  "deviceCode": "device-code",
  "userCode": "ABCD-1234",
  "verificationUri": "https://github.com/login/device",
  "expiresIn": 900,
  "interval": 5
}
```

#### 完成认证

```http
POST /api/github/auth/complete
Content-Type: application/json

{
  "deviceCode": "device-code"
}
```

**响应示例：**

```json
{
  "accessToken": "github-access-token",
  "scope": ["repo", "read:org"],
  "tokenType": "bearer"
}
```

#### 激活账户

```http
POST /api/github/auth/activate
Content-Type: application/json

{
  "accountId": "account-id"
}
```

#### 断开认证

```http
DELETE /api/github/auth
```

#### 获取当前用户信息

```http
GET /api/github/me
```

**响应示例：**

```json
{
  "login": "username",
  "name": "Display Name",
  "avatarUrl": "https://avatars.githubusercontent.com/u/123456",
  "url": "https://github.com/username"
}
```

### Pull Request 管理

#### 获取 PR 状态

```http
GET /api/github/pr/status?directory={path}&branch={branch}&remote={origin}
```

**查询参数：**
- `directory` (必需) - Git 仓库目录
- `branch` (必需) - 分支名称
- `remote` (可选) - 远程仓库名称

**响应示例：**

```json
{
  "connected": true,
  "pr": {
    "number": 123,
    "title": "PR title",
    "state": "open",
    "draft": false,
    "mergeable": true,
    "merged": false,
    "url": "https://github.com/owner/repo/pull/123"
  },
  "checks": {
    "status": "passing",
    "count": 5,
    "passing": 5,
    "failing": 0,
    "pending": 0
  },
  "permissions": {
    "canUpdate": true,
    "canMerge": true
  }
}
```

#### 创建 PR

```http
POST /api/github/pr/create
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "title": "PR title",
  "body": "PR description",
  "head": "feature-branch",
  "base": "main",
  "draft": false
}
```

**响应示例：**

```json
{
  "number": 123,
  "url": "https://github.com/owner/repo/pull/123"
}
```

#### 更新 PR

```http
POST /api/github/pr/update
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "number": 123,
  "title": "Updated title",
  "body": "Updated description"
}
```

#### 合并 PR

```http
POST /api/github/pr/merge
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "number": 123,
  "method": "merge",
  "commitTitle": "Merge title",
  "commitBody": "Merge body"
}
```

**合并方法：** `merge` | `squash` | `rebase`

#### 标记 PR 为准备好审查

```http
POST /api/github/pr/ready
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "number": 123
}
```

### Issue 管理

#### 列出 Issues

```http
GET /api/github/issues/list?owner={owner}&repo={repo}&state={open}
```

**查询参数：**
- `owner` (必需) - 仓库所有者
- `repo` (必需) - 仓库名称
- `state` (可选) - `open` | `closed` | `all`
- `limit` (可选) - 返回数量限制

#### 获取 Issue 详情

```http
GET /api/github/issues/get?owner={owner}&repo={repo}&number={123}
```

#### 获取 Issue 评论

```http
GET /api/github/issues/comments?owner={owner}&repo={repo}&number={123}
```

### Pull Request 列表

```http
GET /api/github/pulls/list?owner={owner}&repo={repo}
```

### PR 上下文

```http
GET /api/github/pulls/context?directory={path}
```

返回当前分支的 PR 上下文信息。

---

## Git API

Git API 提供完整的 Git 仓库操作功能。

### 仓库状态

#### 检查是否为 Git 仓库

```http
GET /api/git/check?directory={path}
```

#### 获取 Git 状态

```http
GET /api/git/status?directory={path}
```

**响应示例：**

```json
{
  "current": "feature-branch",
  "tracking": "origin/feature-branch",
  "ahead": 2,
  "behind": 0,
  "files": [
    {
      "path": "src/file.ts",
      "index": "M",
      "working_dir": "M"
    }
  ],
  "isClean": false,
  "diffStats": {
    "src/file.ts": { "insertions": 10, "deletions": 5 }
  }
}
```

#### 获取远程 URL

```http
GET /api/git/remote-url?directory={path}&remote={origin}
```

### 身份管理

#### 获取全局身份

```http
GET /api/git/global-identity
```

**响应示例：**

```json
{
  "userName": "Your Name",
  "userEmail": "your.email@example.com"
}
```

#### 获取当前身份

```http
GET /api/git/current-identity?directory={path}
```

#### 检查本地身份

```http
GET /api/git/has-local-identity?directory={path}
```

#### 设置身份

```http
POST /api/git/set-identity
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "userName": "Your Name",
  "userEmail": "your.email@example.com"
}
```

#### 身份配置管理

```http
# 获取身份列表
GET /api/git/identities

# 创建身份
POST /api/git/identities

# 更新身份
PUT /api/git/identities/:id

# 删除身份
DELETE /api/git/identities/:id
```

### 差异操作

#### 获取差异

```http
GET /api/git/diff?directory={path}&path={file}&staged={false}&contextLines={3}
```

**响应示例：**

```json
{
  "diff": "diff --git a/src/file.ts b/src/file.ts\n..."
}
```

#### 获取文件差异

```http
GET /api/git/file-diff?directory={path}&path={file}&staged={false}
```

**响应示例：**

```json
{
  "original": "原始内容",
  "modified": "修改后内容",
  "path": "src/file.ts",
  "isBinary": false
}
```

#### 撤销文件

```http
POST /api/git/revert
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "path": "src/file.ts"
}
```

### 分支操作

#### 获取分支列表

```http
GET /api/git/branches?directory={path}
```

**响应示例：**

```json
{
  "all": ["main", "feature-branch", "develop"],
  "current": "feature-branch",
  "branches": {
    "main": {
      "name": "main",
      "current": false,
      "commit": "abc123",
      "label": "main",
      "tracking": "origin/main"
    }
  }
}
```

#### 创建分支

```http
POST /api/git/branches
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "branchName": "new-branch",
  "startPoint": "main"
}
```

#### 检出分支

```http
POST /api/git/checkout
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "branchName": "feature-branch"
}
```

#### 删除分支

```http
DELETE /api/git/branches?directory={path}&branch={branch}&force={false}
```

#### 删除远程分支

```http
DELETE /api/git/remote-branches?directory={path}&branch={branch}&remote={origin}
```

#### 重命名分支

```http
PUT /api/git/branches/rename
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "oldName": "old-branch",
  "newName": "new-branch"
}
```

### 远程操作

#### 获取远程列表

```http
GET /api/git/remotes?directory={path}
```

**响应示例：**

```json
{
  "remotes": [
    { "name": "origin", "url": "git@github.com:owner/repo.git" }
  ]
}
```

#### 拉取更改

```http
POST /api/git/pull
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "remote": "origin",
  "branch": "main"
}
```

#### 推送更改

```http
POST /api/git/push
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "remote": "origin",
  "branch": "feature-branch",
  "setUpstream": true
}
```

#### 获取远程更改

```http
POST /api/git/fetch
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "remote": "origin"
}
```

### 提交操作

#### 创建提交

```http
POST /api/git/commit
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "message": "Commit message",
  "addAll": false,
  "files": ["src/file1.ts", "src/file2.ts"]
}
```

**响应示例：**

```json
{
  "success": true,
  "commit": "abc123def456",
  "branch": "feature-branch",
  "summary": {
    "changes": 2,
    "insertions": 10,
    "deletions": 5
  }
}
```

### 合并与变基

#### 合并分支

```http
POST /api/git/merge
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "branch": "feature-branch"
}
```

#### 中止合并

```http
POST /api/git/merge/abort
Content-Type: application/json

{
  "directory": "/path/to/repo"
}
```

#### 继续合并

```http
POST /api/git/merge/continue
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "message": "Merge commit message"
}
```

#### 变基

```http
POST /api/git/rebase
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "branch": "main"
}
```

#### 中止变基

```http
POST /api/git/rebase/abort
Content-Type: application/json

{
  "directory": "/path/to/repo"
}
```

#### 继续变基

```http
POST /api/git/rebase/continue
Content-Type: application/json

{
  "directory": "/path/to/repo"
}
```

#### 获取冲突详情

```http
GET /api/git/conflict-details?directory={path}
```

**响应示例：**

```json
{
  "operation": "merge",
  "unmergedFiles": [
    {
      "path": "src/file.ts",
      "stage": "both_modified"
    }
  ]
}
```

### 暂存操作

#### 暂存更改

```http
POST /api/git/stash
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "message": "WIP",
  "includeUntracked": false
}
```

#### 弈出暂存

```http
POST /api/git/stash/pop
Content-Type: application/json

{
  "directory": "/path/to/repo"
}
```

### 日志操作

#### 获取提交日志

```http
GET /api/git/log?directory={path}&maxCount={10}&from={main}&to={feature}
```

**响应示例：**

```json
{
  "all": [
    {
      "hash": "abc123",
      "date": "2024-01-01T00:00:00Z",
      "message": "Commit message",
      "author": "Author Name",
      "stats": { "insertions": 10, "deletions": 5 }
    }
  ],
  "latest": { "hash": "abc123", ... },
  "total": 42
}
```

#### 获取提交文件

```http
GET /api/git/commit-files?directory={path}&commitHash={abc123}
```

### 工作树操作

#### 获取工作树列表

```http
GET /api/git/worktrees?directory={path}
```

**响应示例：**

```json
{
  "worktrees": [
    {
      "head": "abc123",
      "name": "feature-worktree",
      "branch": "feature-branch",
      "path": "/path/to/worktree",
      "isLinked": true
    }
  ]
}
```

#### 验证工作树创建

```http
POST /api/git/worktrees/validate
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "mode": "new",
  "branchName": "new-branch"
}
```

#### 创建工作树

```http
POST /api/git/worktrees
Content-Type: application/json

{
  "directory": "/path/to/repo",
  "mode": "new",
  "branchName": "new-branch",
  "startRef": "main"
}
```

#### 删除工作树

```http
DELETE /api/git/worktrees?directory={path}&worktreePath={path}
```

#### 获取工作树类型

```http
GET /api/git/worktree-type?directory={path}
```

### 凭据发现

```http
GET /api/git/discover-credentials?directory={path}
```

---

## 文件系统 API

文件系统 API 提供文件和目录操作功能。

### 基础操作

#### 获取用户主目录

```http
GET /api/fs/home
```

**响应示例：**

```json
{
  "home": "/Users/username"
}
```

#### 创建目录

```http
POST /api/fs/mkdir
Content-Type: application/json

{
  "path": "/path/to/new/directory",
  "recursive": true
}
```

#### 读取文件

```http
GET /api/fs/read?path={path}&encoding={utf8}
```

**响应示例：**

```json
{
  "content": "文件内容",
  "encoding": "utf8"
}
```

#### 获取原始文件

```http
GET /api/fs/raw?path={path}
```

返回原始文件内容（二进制或文本）。

#### 写入文件

```http
POST /api/fs/write
Content-Type: application/json

{
  "path": "/path/to/file.txt",
  "content": "文件内容",
  "encoding": "utf8"
}
```

#### 删除文件

```http
POST /api/fs/delete
Content-Type: application/json

{
  "path": "/path/to/file"
}
```

#### 重命名/移动文件

```http
POST /api/fs/rename
Content-Type: application/json

{
  "from": "/path/from/file.txt",
  "to": "/path/to/file.txt"
}
```

#### 在文件管理器中显示

```http
POST /api/fs/reveal
Content-Type: application/json

{
  "path": "/path/to/file"
}
```

### 目录列表

#### 列出目录内容

```http
GET /api/fs/list?path={path}
```

**响应示例：**

```json
{
  "directory": "/path/to/dir",
  "entries": [
    {
      "name": "file.txt",
      "path": "/path/to/dir/file.txt",
      "isDirectory": false
    },
    {
      "name": "subdir",
      "path": "/path/to/dir/subdir",
      "isDirectory": true
    }
  ]
}
```

### 命令执行

#### 执行命令

```http
POST /api/fs/exec
Content-Type: application/json

{
  "command": "npm",
  "args": ["install"],
  "cwd": "/path/to/project",
  "env": {
    "NODE_ENV": "development"
  }
}
```

**响应示例：**

```json
{
  "jobId": "job-123",
  "pid": 45678
}
```

#### 获取执行结果

```http
GET /api/fs/exec/:jobId
```

**响应示例：**

```json
{
  "running": false,
  "exitCode": 0,
  "stdout": "命令输出",
  "stderr": "错误输出"
}
```

### OpenCode 目录操作

```http
POST /api/opencode/directory
Content-Type: application/json

{
  "directory": "/path/to/project"
}
```

返回项目的 OpenCode 目录结构。

---

## 终端 API

终端 API 提供 PTY（伪终端）会话管理功能。

### 创建终端会话

```http
POST /api/terminal/create
Content-Type: application/json

{
  "cwd": "/path/to/directory",
  "cols": 80,
  "rows": 24,
  "env": {
    "TERM": "xterm-256color"
  }
}
```

**响应示例：**

```json
{
  "sessionId": "session-123",
  "cols": 80,
  "rows": 24,
  "capabilities": {
    "input": {
      "preferred": "ws",
      "transports": ["ws", "http"],
      "ws": {
        "path": "/api/terminal/input-ws",
        "v": 1
      }
    }
  }
}
```

### 终端输出流

```http
GET /api/terminal/:sessionId/stream
```

返回 SSE 事件流，包含终端输出。

**事件类型：**
- `data` - 终端输出数据
- `exit` - 终端退出

### 终端输入

#### HTTP POST 输入

```http
POST /api/terminal/:sessionId/input
Content-Type: text/plain

要发送到终端的文本
```

#### WebSocket 输入

```websocket
WS /api/terminal/input-ws
```

WebSocket 消息格式：

**控制帧（会话绑定）：**
```
0x01 + JSON({
  "type": "bind",
  "sessionId": "session-123"
})
```

**数据帧（用户输入）：**
- 纯文本：直接发送 UTF-8 编码的文本
- 二进制：发送原始字节数据

### 调整终端大小

```http
POST /api/terminal/:sessionId/resize
Content-Type: application/json

{
  "cols": 100,
  "rows": 30
}
```

### 关闭终端会话

```http
DELETE /api/terminal/:sessionId
```

### 重启终端会话

```http
POST /api/terminal/:sessionId/restart
Content-Type: application/json

{
  "cwd": "/new/working/directory"
}
```

### 强制终止终端

```http
POST /api/terminal/force-kill
Content-Type: application/json

{
  "sessionId": "session-123",
  "cwd": "/path/to/directory"
}
```

---

## 会话 API

会话 API 管理与 OpenCode 的 AI 对话会话。

### 发送消息

```http
POST /api/session/:sessionId/message
Content-Type: application/raw

{
  "role": "user",
  "content": "用户消息内容"
}
```

注意：此端点使用 `express.raw()` 中间件，接受原始数据流。

### 会话快照

```http
GET /api/sessions/snapshot
```

**响应示例：**

```json
{
  "sessions": [
    {
      "id": "session-1",
      "title": "会话标题",
      "updatedAt": "2024-01-01T00:00:00Z"
    }
  ]
}
```

### 会话状态

#### 获取所有会话状态

```http
GET /api/sessions/status
```

#### 获取特定会话状态

```http
GET /api/sessions/:id/status
```

**响应示例：**

```json
{
  "id": "session-1",
  "status": "active",
  "mode": "plan"
}
```

### 会话关注

#### 获取会话关注列表

```http
GET /api/sessions/attention
```

#### 获取特定会话关注状态

```http
GET /api/sessions/:id/attention
```

#### 查看会话

```http
POST /api/sessions/:id/view
```

#### 停止查看会话

```http
POST /api/sessions/:id/unview
```

### 消息已发送

```http
POST /api/sessions/:id/message-sent
Content-Type: application/json

{
  "timestamp": 1234567890
}
```

---

## 推送通知 API

推送通知 API 用于管理 Web Push 通知订阅。

### 获取 VAPID 公钥

```http
GET /api/push/vapid-public-key
```

**响应示例：**

```json
{
  "publicKey": "base64-encoded-vapid-public-key"
}
```

### 订阅推送通知

```http
POST /api/push/subscribe
Content-Type: application/json

{
  "subscription": {
    "endpoint": "https://fcm.googleapis.com/...",
    "keys": {
      "p256dh": "key",
      "auth": "auth"
    }
  }
}
```

### 取消订阅

```http
DELETE /api/push/subscribe
```

### 设置可见性

```http
POST /api/push/visibility
Content-Type: application/json

{
  "visible": true
}
```

### 获取可见性状态

```http
GET /api/push/visibility
```

---

## 语音与 TTS API

语音与文本转语音 API 提供语音输入和朗读功能。

### 获取语音令牌

```http
POST /api/voice/token
```

返回用于语音识别的临时令牌。

### TTS 朗读

```http
POST /api/tts/speak
Content-Type: application/json

{
  "text": "要朗读的文本",
  "voice": "coral",
  "speed": 1.0
}
```

**可用声音：**
- `alloy`, `ash`, `ballad`, `coral`, `echo`
- `fable`, `nova`, `onyx`, `sage`, `shimmer`
- `verse`, `marin`, `cedar`

### TTS 摘要

```http
POST /api/tts/summarize
Content-Type: application/json

{
  "text": "长文本内容...",
  "threshold": 200,
  "maxLength": 100
}
```

**响应示例：**

```json
{
  "summary": "摘要文本",
  "summarized": true,
  "originalLength": 500,
  "summaryLength": 80
}
```

### 获取 TTS 状态

```http
GET /api/tts/status
```

**响应示例：**

```json
{
  "available": true,
  "configured": true
}
```

### Say TTS API

#### 获取 Say 状态

```http
GET /api/tts/say/status
```

#### Say 朗读

```http
POST /api/tts/say/speak
Content-Type: application/json

{
  "text": "要朗读的文本"
}
```

---

## 配额 API

配额 API 用于查询各个 AI 提供商的使用配额。

### 获取支持的提供商列表

```http
GET /api/quota/providers
```

**响应示例：**

```json
{
  "providers": [
    "claude",
    "codex",
    "google",
    "github-copilot",
    "kimi-for-coding",
    "nano-gpt",
    "openrouter",
    "zai-coding-plan"
  ]
}
```

### 获取提供商配额

```http
GET /api/quota/:providerId
```

**查询参数：** `providerId` - 提供商 ID

**响应示例：**

```json
{
  "providerId": "claude",
  "providerName": "Claude",
  "ok": true,
  "configured": true,
  "usage": {
    "limit": 1000000,
    "used": 500000,
    "remaining": 500000,
    "percentage": 50
  },
  "fetchedAt": "2024-01-01T00:00:00Z"
}
```

### 删除提供商认证

```http
DELETE /api/provider/:providerId/auth
```

### 获取提供商源信息

```http
GET /api/provider/:providerId/source
```

---

## 数据类型定义

### GitStatusFile

Git 状态中的文件变更信息：

```typescript
interface GitStatusFile {
  path: string;        // 文件路径
  index: string;       // 暂存区状态：M=修改, A=添加, D=删除, R=重命名, C=复制, ??=未跟踪
  working_dir: string; // 工作区状态
}
```

### GitStatus

Git 仓库完整状态：

```typescript
interface GitStatus {
  current: string;                    // 当前分支
  tracking: string | null;            // 跟踪的远程分支
  ahead: number;                      // 领先提交数
  behind: number;                     // 落后提交数
  files: GitStatusFile[];             // 变更文件列表
  isClean: boolean;                   // 工作区是否干净
  diffStats?: Record<string, {        // 差异统计
    insertions: number;
    deletions: number;
  }>;
  mergeInProgress?: GitMergeInProgress | null;
  rebaseInProgress?: GitRebaseInProgress | null;
}
```

### GitHubPullRequestStatus

GitHub PR 状态信息：

```typescript
interface GitHubPullRequestStatus {
  connected: boolean;                 // 是否已连接 GitHub
  pr?: {
    number: number;
    title: string;
    state: 'open' | 'closed' | 'merged';
    draft: boolean;
    mergeable: boolean;
    merged: boolean;
    url: string;
  };
  checks?: {
    status: 'passing' | 'failing' | 'pending';
    count: number;
    passing: number;
    failing: number;
    pending: number;
  };
  permissions?: {
    canUpdate: boolean;
    canMerge: boolean;
  };
}
```

### TerminalSession

终端会话信息：

```typescript
interface TerminalSession {
  sessionId: string;
  cols: number;
  rows: number;
  capabilities?: {
    input?: {
      preferred?: 'ws' | 'http';
      transports?: Array<'ws' | 'http'>;
      ws?: {
        path: string;
        v?: number;
        enc?: string;
      };
    };
  };
}
```

### DirectoryListResult

目录列表结果：

```typescript
interface DirectoryListResult {
  directory: string;
  entries: Array<{
    name: string;
    path: string;
    isDirectory: boolean;
  }>;
}
```

### SkillsCatalogItem

技能目录项：

```typescript
interface SkillsCatalogItem {
  repoSource: string;                 // GitHub 仓库来源
  repoSubpath?: string;               // 仓库子路径
  skillDir: string;                   // 技能目录
  skillName: string;                  // 技能名称
  frontmatterName?: string;           // SKILL.md 中的名称
  description?: string;               // 技能描述
  installable: boolean;               // 是否可安装
  warnings?: string[];                // 警告信息
}
```

---

## 使用示例

### JavaScript/TypeScript 客户端示例

```typescript
// 创建 Git 提交
async function createCommit(directory: string, message: string) {
  const response = await fetch('/api/git/commit', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ directory, message, addAll: true })
  });

  if (!response.ok) {
    throw new Error(`Commit failed: ${response.statusText}`);
  }

  return await response.json();
}

// 监听 Server-Sent Events
function listenToSessionEvents(sessionId: string) {
  const eventSource = new EventSource(`/api/event?sessionId=${sessionId}`);

  eventSource.addEventListener('session.message', (event) => {
    const data = JSON.parse(event.data);
    console.log('New message:', data);
  });

  eventSource.addEventListener('session.error', (event) => {
    const error = JSON.parse(event.data);
    console.error('Session error:', error);
  });

  return eventSource;
}

// 创建终端会话并连接
async function createTerminal(cwd: string) {
  const createResponse = await fetch('/api/terminal/create', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ cwd, cols: 80, rows: 24 })
  });

  const session = await createResponse.json();

  // 连接 SSE 输出流
  const eventSource = new EventSource(`/api/terminal/${session.sessionId}/stream`);
  eventSource.addEventListener('data', (event) => {
    console.log('Terminal output:', event.data);
  });

  return session;
}
```

### cURL 示例

```bash
# 获取 Git 状态
curl "http://localhost:3001/api/git/status?directory=$PWD"

# 创建 Git 提交
curl -X POST http://localhost:3001/api/git/commit \
  -H "Content-Type: application/json" \
  -d '{"directory": "/path/to/repo", "message": "Commit message", "addAll": true}'

# 获取 GitHub PR 状态
curl "http://localhost:3001/api/github/pr/status?directory=$PWD&branch=feature-branch"

# 创建终端会话
curl -X POST http://localhost:3001/api/terminal/create \
  -H "Content-Type: application/json" \
  -d "{\"cwd\": \"$PWD\", \"cols\": 80, \"rows\": 24}"

# 列出目录内容
curl "http://localhost:3001/api/fs/list?path=$PWD"
```

---

## 错误处理

所有 API 错误返回统一的格式：

```json
{
  "error": "错误描述消息",
  "code": "ERROR_CODE"  // 可选
}
```

常见错误代码：
- `NOT_FOUND` - 资源不存在
- `UNAUTHORIZED` - 未授权访问
- `INVALID_INPUT` - 输入参数无效
- `GIT_ERROR` - Git 操作失败
- `GITHUB_ERROR` - GitHub API 错误
- `FS_ERROR` - 文件系统操作错误

---

## 相关文档

- [项目概览](./01-project-overview.md)
- [技术栈](./02-tech-stack.md)
- [架构设计](./03-architecture.md)
- [代码结构](./04-code-structure.md)
- [开发环境搭建](./05-development-setup.md)

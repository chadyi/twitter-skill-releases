# Twitter Skill

[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [한국어](README.ko.md)

让 Codex、Claude Code 和其他 AI Agent 通过用户自己的本地登录态操作 X/Twitter。

Twitter Skill 由 Windows/macOS 桌面客户端、JSON CLI 和标准 Agent Skill 组成。Agent 可以搜索 X、使用多个已连接账号、上传图片、发布帖子和回复、删除帖子，同时不会获得用户的 X Cookie。

## 能力

| 能力 | 说明 |
| --- | --- |
| 搜索 | 支持 X 搜索语法、返回数量、结果模式和游标分页 |
| 账号 | 列出已连接账号，使用默认账号或明确指定账号 |
| 发布 | 发布文字、回复，以及附带最多四张图片的帖子 |
| 媒体 | 上传 JPEG、PNG 和 WebP 图片 |
| 管理 | 删除帖子并检查本地账号状态 |
| Agent 接入 | 为 Codex、Claude Code、Cursor 及其他受 `skills` CLI 支持的 Agent 安装同一份标准 Skill |

## 使用条件

1. 从 [GitHub Releases](https://github.com/chadyi/twitter-skill-releases/releases/latest) 安装最新版 Twitter Skill 桌面客户端。
2. 打开客户端，连接至少一个 X 账号，并让程序在系统托盘中保持运行。
3. 安装 [Node.js](https://nodejs.org/)，确保系统可以执行 `npx`。

## 安装 Agent Skill

下面两种方式安装的是同一份全局 Skill，任选其一。

### 自己执行

在终端中运行：

```bash
npx skills add chadyi/twitter-skill-releases -s twitter-skill -g -y
```

### 复制给 Agent 执行

把下面这段话发送给 Codex、Claude Code 或其他受支持的 Agent：

```text
请从 https://github.com/chadyi/twitter-skill-releases 全局安装 Twitter Skill，执行：
npx skills add chadyi/twitter-skill-releases -s twitter-skill -g -y

安装完成后，检查 twitter-skill 是否已经安装，读取它的 SKILL.md，并确认本地 Twitter Skill 桌面客户端是否就绪。不要索取、输出或保存 X Cookie。
```

手动检查安装结果：

```bash
npx skills list -g
```

以后更新 Skill：

```bash
npx skills update twitter-skill -g
```

## 使用方式

安装后，直接向 Agent 描述任务：

```text
搜索 X 上关于 GPT Image 2 的最新内容，返回 20 条。
```

```text
使用我的默认 X 账号，附带这张图片发布“hello”。
```

```text
用一段简短总结回复这条 X 帖子。
```

Agent 会读取 Skill 并调用本地 CLI。CLI 使用结构化 JSON 返回结果和错误，方便 Agent 稳定处理。

## 工作方式

```text
Codex / Claude Code / 其他 Agent
  -> 已安装的 twitter-skill/SKILL.md
  -> 本地 Twitter Skill CLI
  -> 正在运行的桌面托盘进程
  -> 服务器下发的短期请求方案
  -> 使用用户本地 Electron 登录态和网络请求 X
```

- X 登录态保存在桌面客户端隔离的本地账号分区中。
- X 请求从用户设备发出，不经过共享服务器 IP 请求 X。
- Skill 只包含 CLI 使用说明，不包含 Cookie 或私有的 X 请求实现细节。
- 公开仓库只保存 Agent Skill 和发行文件，客户端及服务端源码仍为私有。

Twitter Skill 使用用户已经登录的 X 网页会话，不使用 X 官方 API。

## 桌面客户端下载与更新

从 [Releases](https://github.com/chadyi/twitter-skill-releases/releases/latest) 下载最新版本。

| 平台 | 安装包 |
| --- | --- |
| Windows x64 | `Twitter-Skill-<version>-x64.exe` |
| macOS Universal | `Twitter-Skill-<version>-universal.dmg` |

每个 Release 还包含自动更新元数据、blockmap、macOS 更新用 ZIP 和 `SHA256SUMS.txt`。桌面客户端会从这个公开仓库检查更新，并在后台下载对应平台的文件。

## 常见问题

### Agent 找不到 `twitter-skill`

运行 `npx skills list -g`，重新安装 Skill，然后重启 Agent，让它重新加载全局 Skills。

### 本地 CLI 不可用

至少启动一次 Twitter Skill 桌面客户端，并让它在托盘中运行。客户端启动时会生成本地 CLI 启动器。

### 本地 X 登录已过期

打开桌面客户端，重新连接对应账号。不要把 X Cookie 粘贴到 Agent 或终端中。

### 发帖请求超时

重试之前先去 X 检查账号和帖子。超时的发布请求可能已经执行成功。

## 仓库内容

```text
skills/twitter-skill/SKILL.md  标准 Agent Skill
GitHub Releases                Windows/macOS 安装包和更新元数据
```

应用问题和发行问题可以通过 [GitHub Issues](https://github.com/chadyi/twitter-skill-releases/issues) 反馈。

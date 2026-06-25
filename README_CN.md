<p align="center">
  <img src="https://raw.githubusercontent.com/yehuoshun/yuque-ai-mcp/main/assets/banner.png" width="800" alt="yuque-ai-skills" />
</p>

<h1 align="center">yuque-ai-skills</h1>
<p align="center">
  <b>语雀 AI Agent Skill 层 — 63 个使用指导，零 MCP 开销</b>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-green?style=flat-square" alt="license" /></a>
  <a href="https://github.com/yehuoshun/yuque-ai-mcp"><img src="https://img.shields.io/badge/MCP-yuque--ai--mcp-blue?style=flat-square" alt="MCP" /></a>
  <a href="https://github.com/yehuoshun/yuque-ai-skills"><img src="https://img.shields.io/badge/skills-63%20guides-orange?style=flat-square" alt="skills" /></a>
</p>

<p align="center">
  <a href="README.md">English</a>
</p>

---

[yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp) 的 **Skill 层**。提供 63 个结构化使用指导，教会 AI Agent 如何调用每个语雀 OpenAPI 端点——无需 MCP 运行时。任何能读 Markdown 文件并发 HTTP 请求的 AI Agent 都能用。

## 目录

- [什么是 Skill？](#什么是-skill)
- [快速开始](#快速开始)
- [Skill 结构](#skill-结构)
- [领域概览](#领域概览)
- [相关项目](#相关项目)
- [许可](#许可)

## 什么是 Skill？

**Skill** 是一个 Markdown 文件，教 AI Agent 如何使用特定工具。包含：

- **工具名和描述** — 工具做什么
- **端点** — API URL 和 HTTP 方法
- **参数** — 必填和可选字段及类型
- **示例** — 真实使用示例
- **注意事项** — 边界情况、速率限制、坑点

Skill 是**纯文档**——没有代码、没有运行时、没有依赖。任何能读文件并发 HTTP 请求的 AI Agent 都能用。

### Skill vs MCP

| | MCP (yuque-ai-mcp) | Skill (yuque-ai-skills) |
|---|---|---|
| **运行时** | 需要 Node.js MCP 服务器 | 零运行时——仅 Markdown 文件 |
| **依赖** | @modelcontextprotocol/sdk, Zod 等 | 无 |
| **传输** | stdio / HTTP SSE | 文件系统 |
| **Agent 支持** | 仅 MCP 兼容 Agent | 任何能读文件的 Agent |
| **开销** | 服务器进程、内存、端口 | 零 |
| **更新** | 重启服务器 | 更新文件即可 |

## 快速开始

### AI Agent 使用

1. 读取 `SKILL.md` — 全部 63 个工具的主索引
2. 对于需要的每个工具，读取 `references/api/` 中的对应文件
3. 根据 Skill 中的端点和参数直接调用语雀 API

```bash
# 示例：根据 Skill 指导创建文档
curl -X POST "https://www.yuque.com/api/v2/repos/{book_id}/docs" \
  -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"title": "我的文档", "body": "Hello world", "format": "markdown"}'
```

### 人类使用

```bash
git clone https://github.com/yehuoshun/yuque-ai-skills.git
# 将 AI Agent 的 skill 目录指向此仓库
# Agent 会自动读取 SKILL.md 和参考文件
```

## Skill 结构

```
yuque-ai-skills/
├── SKILL.md                    # 主索引——全部 63 个工具速查表
├── README.md                   # 英文文档
├── README_CN.md                # 本文件
└── references/
    └── api/                    # 每个 API 域一个文件
        ├── doc_api.md          # 15 个文档工具
        ├── repo_api.md         # 8 个知识库工具
        ├── toc_api.md          # 3 个目录工具
        ├── search_api.md       # 3 个搜索工具
        ├── user_api.md         # 3 个用户工具
        ├── group_api.md        # 3 个团队工具
        ├── statistic_api.md    # 4 个统计工具
        ├── note_api.md         # 4 个小记工具
        ├── recycle_api.md      # 3 个回收站工具
        ├── upload_api.md       # 1 个上传工具
        ├── board_api.md        # 3 个画板工具
        ├── mine_api.md         # 2 个个人工具
        ├── rss_api.md          # 3 个 RSS 工具
        ├── crawler_api.md      # 4 个爬虫工具
        ├── kv_api.md           # 4 个 KV 工具
        └── errors.md           # 错误码参考
```

## 领域概览

| 域 | 工具数 | 说明 |
|--------|-------|-------------|
| **doc** | 15 | 文档 CRUD、版本管理、Diff、导入导出、跨库复制 |
| **repo** | 8 | 知识库 CRUD、批量操作、跨库复制、全量导出 |
| **toc** | 3 | 目录——获取、更新、批量更新 |
| **search** | 3 | 通用搜索、RAG 搜索、Cookie Web 搜索 |
| **user** | 3 | 用户信息、心跳、团队列表 |
| **group** | 3 | 成员管理、角色变更 |
| **statistic** | 4 | 团队/成员/知识库/文档统计 |
| **note** | 4 | 小记 CRUD + 软删除 |
| **recycle** | 3 | 回收站——列表、恢复、彻底删除 |
| **upload** | 1 | 文件上传到语雀 CDN |
| **board** | 3 | 思维导图、流程图、架构图 |
| **mine** | 2 | 书架列表、编辑中心 |
| **rss** | 3 | RSS 抓取、去重、定时策略 |
| **crawler** | 4 | 网页爬取、CSS 提取、去重写入 |
| **kv** | 4 | 键值存储（增量分片） |
| **合计** | **63** | |

## 相关项目

- **[yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp)** — 同 63 个工具的 MCP Server，适用于 MCP 兼容 Agent
- 两个仓库保持同步——MCP 新增工具后同步更新此处的 Skill 文档

## 许可

MIT

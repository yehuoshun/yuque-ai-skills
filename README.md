# yuque-ai-skills / 语雀 AI Skills

语雀 Skill 层 — 工具使用指导、场景模式、最佳实践。

Yuque Skill layer — usage guides, patterns, and best practices.

> **📌 输出裁剪 / Output Trimming**：所有工具默认返回精简字段（去除 `_serializer`、`type` 等元数据）。传 `raw=true` 可获取全量原始 JSON。
> All tools return trimmed fields by default. Pass `raw=true` for full raw JSON.

> ⚠️ 本仓库不包含可执行代码，只做用法文档。MCP 工具实现在 [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp)。
> This repo contains no executable code — only usage docs. MCP implementation at [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp).

## 目录结构 / Structure

```
skills/
├── user/            # 用户信息 / User（3）
├── search/          # 搜索 / Search（2）
├── group/           # 团队管理 / Group（3）
├── doc/             # 文档 CRUD / Doc（14）
├── toc/             # 目录管理 / TOC（3）
├── repo/            # 知识库管理 / Repo（8）
├── statistic/       # 统计数据 / Statistics（4）
├── note/            # 小记 / Note（4）
├── recycle/         # 回收站 / Recycle（3）
├── upload/          # 文件上传 / Upload（1）
├── board/           # 画板资源 / Board（3）
├── rss/             # RSS 抓取 / RSS（3）
├── crawler/         # 网页爬虫 / Crawler（4）
├── mine/            # 个人数据 / Mine（2）
└── kv/              # KV 键值存储 / KV（4）

references/api/      # API 参考文档 / API reference（17 domains）
```

## 覆盖范围 / Coverage

| 域 / Domain | 工具数 / Tools | 说明 / Description |
|-----|--------|------|
| user | 3 | 用户信息、心跳、团队列表 / User info, heartbeat, group list |
| search | 2 | 通用搜索 + RAG 增强搜索 / General search + RAG-enhanced search |
| doc | 14 | 文档 CRUD + 版本管理 + 批量获取 + 导入导出 + 跨库复制 + URL导入 + 文件导入 |
| repo | 8 | 知识库 CRUD + 批量获取 + 导出 + 跨库复制 |
| group | 3 | 团队成员管理 / Group member management |
| toc | 3 | 目录获取 + 更新 + 批量更新 / TOC get/update/batch |
| statistic | 4 | 团队/成员/知识库/文档统计 / Group/member/repo/doc statistics |
| note | 4 | 小记 CRUD / Note CRUD |
| recycle | 3 | 回收站列表/恢复/删除 / Recycle list/restore/destroy |
| upload | 1 | 文件上传 / File upload |
| board | 3 | 画板资源（思维导图/流程图/架构图） / Board (mindmap/flowchart/architecture) |
| rss | 3 | RSS 抓取（数据源列表 + 抓取写入 + 定时策略分析） |
| crawler | 4 | 网页爬虫（抓取 + CSS提取 + 去重写入 + 定时策略，KV去重） |
| mine | 2 | 个人 Web API（书架列表 + 编辑中心，Cookie 认证） |
| kv | 4 | KV 键值存储（增量分片，需指定 domain 定位 kv_slugs，单文档上限 250KB） |
| **合计 / Total** | **61** | |

## 配套仓库 / Related

- [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp) — MCP Server 实现（TypeScript，HTTP SSE + stdio 双模式）
- 两个仓库保持功能同步，MCP 新增工具 → Skills 同步新增 skill 文件
- Both repos stay in sync — new MCP tools → new skill files
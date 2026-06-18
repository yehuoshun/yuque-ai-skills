# yuque-ai-skills

语雀 Skill 层 — 工具使用指导、场景模式、最佳实践。

> **📌 输出裁剪**：所有工具默认返回精简字段（去除 `_serializer`、`type` 等元数据）。传 `raw=true` 可获取全量原始 JSON。

> ⚠️ 本仓库不包含可执行代码，只做用法文档。MCP 工具实现在 [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp)。

## 目录结构

```
skills/
├── search/          # 搜索
│   ├── search.md    # 通用搜索
│   └── rag-search.md   # RAG 增强搜索
├── doc/             # 文档 CRUD + 版本管理 + 批量获取 + 嵌入 + 导入导出 + 跨库复制 + URL导入（14 个）
├── repo/            # 知识库管理 + 导出 + 跨库复制（8 个）
├── group/           # 团队管理（3 个）
├── toc/             # 目录管理（2 个）
├── statistic/       # 统计数据（4 个）
├── note/            # 小记（4 个）
├── recycle/         # 回收站（3 个）
├── upload/          # 文件上传（1 个）
├── board/           # 画板资源（3 个）
├── rss/             # RSS 抓取（3 个）
├── kv/              # KV 键值存储（4 个）
└── user/            # 用户信息（3 个）

references/api/      # API 参考文档（按域拆分）
```

## 覆盖范围

| 域 | 工具数 | 说明 |
|-----|--------|------|
| user | 3 | 用户信息、心跳、团队列表 |
| search | 2 | 通用搜索 + RAG 增强搜索 |
| doc | 14 | 文档 CRUD + 版本管理 + 批量获取 + 导入导出 + 跨库复制 + URL导入 + 文件导入 |
| repo | 8 | 知识库 CRUD + 批量获取 + 导出 + 跨库复制 |
| group | 3 | 团队成员管理 |
| toc | 2 | 目录获取 + 更新 |
| statistic | 4 | 团队/成员/知识库/文档统计 |
| note | 4 | 小记 CRUD |
| recycle | 3 | 回收站列表/恢复/删除 |
| upload | 1 | 文件上传 |
| board | 3 | 画板资源（思维导图/流程图/架构图） |
| rss | 3 | RSS 抓取（数据源列表 + 抓取写入 + 定时策略分析，自动去重+目录，支持 id/book_id/namespace） |
| crawler | 5 | 网页爬虫（抓取 + CSS提取 + 一站式写入 + 博客园专用 + 定时策略，KV去重） |
| mine | 1 | 个人 Web API（知识库分组/书架列表，Cookie 认证） |
| kv | 4 | KV 键值存储（增量分片，config 记录 {book_id, docs:[doc_id]}，单文档上限 250KB） |
| **合计** | **61** | |

## 配套仓库

- [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp) — MCP Server 实现（TypeScript，HTTP SSE + stdio 双模式）
- 两个仓库保持功能同步，MCP 新增工具 → Skills 同步新增 skill 文件
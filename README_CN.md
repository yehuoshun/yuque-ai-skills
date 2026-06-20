# yuque-ai-skills / 语雀 AI Skills

语雀 Skill 层 — 工具使用指导、场景模式、最佳实践。

[English Documentation](README.md)

> **📌 输出裁剪**：所有工具默认返回精简字段。传 `raw=true` 可获取全量原始 JSON。

> ⚠️ 本仓库不包含可执行代码，只做用法文档。MCP 工具实现在 [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp)。

## 目录结构

```
skills/
├── user/            # 用户信息（3）
├── search/          # 搜索（2）
├── group/           # 团队管理（3）
├── doc/             # 文档 CRUD（14）
├── toc/             # 目录管理（3）
├── repo/            # 知识库管理（8）
├── statistic/       # 统计数据（4）
├── note/            # 小记（4）
├── recycle/         # 回收站（3）
├── upload/          # 文件上传（1）
├── board/           # 画板资源（3）
├── rss/             # RSS 抓取（3）
├── crawler/         # 网页爬虫（4）
├── mine/            # 个人数据（2）
└── kv/              # KV 键值存储（4）

references/api/      # API 参考文档（17 个域）
```

## 覆盖范围

| 域 | 工具数 | 说明 |
|-----|--------|------|
| user | 3 | 用户信息、心跳、团队列表 |
| search | 2 | 通用搜索 + RAG 增强搜索 |
| doc | 14 | 文档 CRUD + 版本管理 + 批量获取 + 导入导出 + 跨库复制 |
| repo | 8 | 知识库 CRUD + 批量获取 + 导出 + 跨库复制 |
| group | 3 | 团队成员管理 |
| toc | 3 | 目录获取 + 更新 + 批量更新 |
| statistic | 4 | 团队/成员/知识库/文档统计 |
| note | 4 | 小记 CRUD |
| recycle | 3 | 回收站列表/恢复/删除 |
| upload | 1 | 文件上传 |
| board | 3 | 画板资源（思维导图/流程图/架构图） |
| rss | 3 | RSS 抓取（数据源列表 + 抓取写入 + 定时策略分析） |
| crawler | 4 | 网页爬虫（抓取 + CSS提取 + 去重写入 + 定时策略） |
| mine | 2 | 个人 Web API（书架列表 + 编辑中心，Cookie 认证） |
| kv | 4 | KV 键值存储（增量分片，单文档 250KB 上限） |
| **合计** | **61** | |

## 配套仓库

- [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp) — MCP Server 实现（TypeScript，HTTP SSE + stdio 双模式）
- 两个仓库保持功能同步，MCP 新增工具 → Skills 同步新增 skill 文件
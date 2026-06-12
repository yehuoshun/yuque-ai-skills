# yuque-ai-skills

语雀 Skill 层 — 工具使用指导、场景模式、最佳实践。

> ⚠️ 本仓库不包含可执行代码，只做用法文档。MCP 工具实现在 [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp)。

## 目录结构

```
skills/
├── search/          # 搜索
│   ├── search.md    # 通用搜索
│   └── hyde-search.md  # HyDE 降级搜索
├── doc/             # 文档 CRUD（7 个）
├── repo/            # 知识库管理（5 个）
├── group/           # 团队管理（3 个）
├── toc/             # 目录管理（2 个）
├── statistic/       # 统计数据（4 个）
├── note/            # 小记（4 个）
├── recycle/         # 回收站（3 个）
├── upload/          # 文件上传（1 个）
├── board/           # 画板资源（3 个）
└── user/            # 用户信息（3 个）

references/api/      # API 参考文档（按域拆分）
```

## 覆盖范围

| 域 | 工具数 | 说明 |
|-----|--------|------|
| user | 3 | 用户信息、心跳、团队列表 |
| search | 2 | 通用搜索 + HyDE 降级搜索 |
| doc | 8 | 文档 CRUD + 版本管理 |
| repo | 5 | 知识库 CRUD |
| group | 3 | 团队成员管理 |
| toc | 2 | 目录获取 + 更新 |
| statistic | 4 | 团队/成员/知识库/文档统计 |
| note | 4 | 小记 CRUD |
| recycle | 3 | 回收站列表/恢复/删除 |
| upload | 1 | 文件上传 |
| board | 3 | 画板资源（思维导图/流程图/架构图） |
| **合计** | **38** | |

## 配套仓库

- [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp) — MCP Server 实现（TypeScript，HTTP SSE + stdio 双模式）
- 两个仓库保持功能同步，MCP 新增工具 → Skills 同步新增 skill 文件
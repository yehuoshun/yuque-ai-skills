# yuque-ai-skills

Yuque Skill layer — usage guides, patterns, and best practices.

[中文文档 / Chinese Documentation](README_CN.md)

> **📌 Output Trimming**: All tools return trimmed fields by default. Pass `raw=true` for full raw JSON.

> ⚠️ This repo contains no executable code — only usage docs. MCP implementation at [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp).

## Structure

```
skills/
├── user/            # User (3)
├── search/          # Search (2)
├── group/           # Group (3)
├── doc/             # Doc (14)
├── toc/             # TOC (3)
├── repo/            # Repo (8)
├── statistic/       # Statistics (4)
├── note/            # Note (4)
├── recycle/         # Recycle (3)
├── upload/          # Upload (1)
├── board/           # Board (3)
├── rss/             # RSS (3)
├── crawler/         # Crawler (4)
├── mine/            # Mine (2)
└── kv/              # KV Store (4)

references/api/      # API reference docs (17 domains)
```

## Coverage

| Domain | Tools | Description |
|-----|--------|------|
| user | 3 | User info, heartbeat, group list |
| search | 2 | General search + RAG-enhanced search |
| doc | 14 | Doc CRUD + versions + batch + import/export + cross-book copy |
| repo | 8 | Repo CRUD + batch + export + cross-book copy |
| group | 3 | Group member management |
| toc | 3 | TOC get/update/batch |
| statistic | 4 | Group/member/repo/doc statistics |
| note | 4 | Note CRUD |
| recycle | 3 | Recycle list/restore/destroy |
| upload | 1 | File upload |
| board | 3 | Board (mindmap/flowchart/architecture diagram) |
| rss | 3 | RSS fetch (source list + fetch + schedule analysis) |
| crawler | 4 | Web crawler (fetch + CSS extract + dedup save + schedule) |
| mine | 2 | Web API (book stacks + editor center, Cookie auth) |
| kv | 4 | KV store (incremental sharding, 250KB limit per doc) |
| **Total** | **61** | |

## Related

- [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp) — MCP Server implementation (TypeScript, HTTP SSE + stdio dual mode)
- Both repos stay in sync — new MCP tools → new skill files
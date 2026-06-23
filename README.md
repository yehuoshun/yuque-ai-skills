<p align="center">
  <h1 align="center">yuque-ai-skills</h1>
  <p align="center">
    <b>62 usage guides for the 62-tool Yuque MCP Server</b>
  </p>
</p>

<p align="center">
  <a href="https://github.com/yehuoshun/yuque-ai-skills"><img src="https://img.shields.io/badge/guides-62-orange" alt="guides" /></a>
  <a href="https://github.com/yehuoshun/yuque-ai-mcp"><img src="https://img.shields.io/badge/mcp-v2.7.8-blue" alt="mcp" /></a>
</p>

<p align="center">
  <a href="README_CN.md">中文文档</a>
</p>

---

Skill layer for [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp) — usage guides, patterns, and best practices for AI agents.

> ⚠️ This repo contains no executable code — only usage docs.

## Coverage

| Domain | Tools | Description |
|--------|-------|-------------|
| doc | 15 | CRUD, versions, diff, batch, import/export, cross-book copy, resource download |
| repo | 8 | CRUD, batch, cross-book copy, full export |
| toc | 3 | Get, update, batch update |
| search | 2 | General + RAG-enhanced search |
| user | 3 | User info, heartbeat, groups |
| group | 3 | Member management |
| statistic | 4 | Group/member/repo/doc statistics |
| note | 4 | CRUD + soft-delete |
| recycle | 3 | List, restore, destroy |
| upload | 1 | File upload to CDN |
| board | 3 | Mindmap, flowchart, architecture diagram |
| mine | 2 | Book stacks, editor center |
| rss | 3 | Source list, fetch + dedup, schedule |
| crawler | 4 | Fetch, CSS extract, dedup save, schedule |
| kv | 4 | Incremental sharding key-value store |
| **Total** | **62** | |

## Structure

```
skills/           # 15 domain directories, 62 skill files
references/api/   # API reference docs (17 domains)
SKILL.md          # Master index with endpoint table
```

## Related

- [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp) — MCP Server implementation
- Both repos kept in sync — new MCP tools → new skill files

## License

MIT
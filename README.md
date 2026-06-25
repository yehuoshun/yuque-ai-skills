<p align="center">
  <img src="https://raw.githubusercontent.com/yehuoshun/yuque-ai-mcp/main/assets/banner.png" width="800" alt="yuque-ai-skills" />
</p>

<h1 align="center">yuque-ai-skills</h1>
<p align="center">
  <b>AI Agent Skill Layer for Yuque — 63 usage guides, zero MCP overhead</b>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-green?style=flat-square" alt="license" /></a>
  <a href="https://github.com/yehuoshun/yuque-ai-mcp"><img src="https://img.shields.io/badge/MCP-yuque--ai--mcp-blue?style=flat-square" alt="MCP" /></a>
  <a href="https://github.com/yehuoshun/yuque-ai-skills"><img src="https://img.shields.io/badge/skills-63%20guides-orange?style=flat-square" alt="skills" /></a>
</p>

<p align="center">
  <a href="README_CN.md">中文文档</a>
</p>

---

The **Skill layer** for [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp). Provides 63 structured usage guides that teach AI agents how to call every Yuque OpenAPI endpoint — no MCP runtime required. Works with any AI agent that can read markdown files and make HTTP requests.

## Table of Contents

- [What is a Skill?](#what-is-a-skill)
- [Quick Start](#quick-start)
- [Skill Structure](#skill-structure)
- [Domain Overview](#domain-overview)
- [Related Projects](#related-projects)
- [License](#license)

## What is a Skill?

A **Skill** is a markdown file that teaches an AI agent how to use a specific tool. It contains:

- **Tool name & description** — what the tool does
- **Endpoint** — the API URL and HTTP method
- **Parameters** — required and optional fields with types
- **Examples** — real-world usage examples
- **Notes** — edge cases, rate limits, gotchas

Skills are **pure documentation** — no code, no runtime, no dependencies. Any AI agent that can read files and make HTTP requests can use them.

### Skill vs MCP

| | MCP (yuque-ai-mcp) | Skill (yuque-ai-skills) |
|---|---|---|
| **Runtime** | Requires Node.js MCP server | Zero runtime — just markdown files |
| **Dependencies** | @modelcontextprotocol/sdk, Zod, etc. | None |
| **Transport** | stdio / HTTP SSE | File system |
| **Agent support** | MCP-compatible agents only | Any agent that reads files |
| **Overhead** | Server process, memory, port | Zero |
| **Updates** | Restart server | Just update files |

## Quick Start

### For AI Agents

1. Read `SKILL.md` — the master index of all 63 tools
2. For each tool you need, read the corresponding file in `references/api/`
3. Call the Yuque API directly using the endpoint and parameters from the skill

```bash
# Example: create a document using the skill guide
curl -X POST "https://www.yuque.com/api/v2/repos/{book_id}/docs" \
  -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"title": "My Doc", "body": "Hello world", "format": "markdown"}'
```

### For Humans

```bash
git clone https://github.com/yehuoshun/yuque-ai-skills.git
# Point your AI agent's skill directory to this repo
# The agent will read SKILL.md and reference files automatically
```

## Skill Structure

```
yuque-ai-skills/
├── SKILL.md                    # Master index — all 63 tools with quick reference
├── README.md                   # This file
├── README_CN.md                # 中文文档
└── references/
    └── api/                    # One file per API domain
        ├── doc_api.md          # 15 document tools
        ├── repo_api.md         # 8 repository tools
        ├── toc_api.md          # 3 TOC tools
        ├── search_api.md       # 3 search tools
        ├── user_api.md         # 3 user tools
        ├── group_api.md        # 3 group tools
        ├── statistic_api.md    # 4 statistics tools
        ├── note_api.md         # 4 note tools
        ├── recycle_api.md      # 3 recycle tools
        ├── upload_api.md       # 1 upload tool
        ├── board_api.md        # 3 board tools
        ├── mine_api.md         # 2 mine tools
        ├── rss_api.md          # 3 RSS tools
        ├── crawler_api.md      # 4 crawler tools
        ├── kv_api.md           # 4 KV tools
        └── errors.md           # Error code reference
```

## Domain Overview

| Domain | Tools | Description |
|--------|-------|-------------|
| **doc** | 15 | Document CRUD, versions, diff, import/export, cross-book copy |
| **repo** | 8 | Repository CRUD, batch operations, cross-book copy, full export |
| **toc** | 3 | Table of contents — get, update, batch update |
| **search** | 3 | General search, RAG search, Cookie web search |
| **user** | 3 | User info, heartbeat, group list |
| **group** | 3 | Member management, role changes |
| **statistic** | 4 | Group/member/repo/doc statistics |
| **note** | 4 | Quick notes CRUD + soft delete |
| **recycle** | 3 | Recycle bin — list, restore, destroy |
| **upload** | 1 | File upload to Yuque CDN |
| **board** | 3 | Mindmap, flowchart, architecture diagram |
| **mine** | 2 | Book stacks, editor center |
| **rss** | 3 | RSS feed fetch, dedup, schedule analysis |
| **crawler** | 4 | Web crawl, CSS extract, dedup save |
| **kv** | 4 | Key-value store with incremental sharding |
| **Total** | **63** | |

## Related Projects

- **[yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp)** — MCP Server with the same 63 tools, for MCP-compatible agents
- Both repos are kept in sync — new tools added to MCP are documented here

## License

MIT

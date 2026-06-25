<p align="center">
  <img src="https://raw.githubusercontent.com/yehuoshun/yuque-ai-skills/main/assets/banner.png" width="800" alt="yuque-ai-skills" />
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
2. For each tool you need, read the corresponding file in `skills/{domain}/`
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
├── AGENT-INSTALL.md            # MCP server install guide
├── assets/
│   └── banner.png
├── config/
│   └── config.example.json
├── .github/
│   └── workflows/
│       └── dingtalk-notify.yml
├── skills/
│   ├── common/                 # Shared docs (reused across domains)
│   │   ├── auth.md
│   │   ├── config.md
│   │   ├── conventions.md
│   │   └── errors.md
│   ├── doc/                    # 15 document tools
│   │   ├── list-docs.md
│   │   ├── create-doc.md
│   │   ├── get-doc.md
│   │   ├── update-doc.md
│   │   ├── delete-doc.md
│   │   ├── batch-get-docs.md
│   │   ├── get-doc-versions.md
│   │   ├── get-doc-version-detail.md
│   │   ├── diff-doc-versions.md
│   │   ├── copy-doc.md
│   │   ├── export-doc.md
│   │   ├── export-resources.md
│   │   ├── import-url.md
│   │   ├── import-file.md
│   │   └── embed-url.md
│   ├── repo/                   # 8 repository tools
│   │   ├── list-repos.md
│   │   ├── create-repo.md
│   │   ├── get-repo.md
│   │   ├── update-repo.md
│   │   ├── delete-repo.md
│   │   ├── batch-get-repos.md
│   │   ├── copy-repo.md
│   │   └── export-repo.md
│   ├── toc/                    # 3 TOC tools
│   │   ├── get-toc.md
│   │   ├── update-toc.md
│   │   └── batch-update-toc.md
│   ├── search/                 # 3 search tools
│   │   ├── search.md
│   │   ├── rag-search.md
│   │   └── web-search.md
│   ├── user/                   # 3 user tools
│   │   ├── hello.md
│   │   ├── get-user.md
│   │   └── get-user-groups.md
│   ├── group/                  # 3 group tools
│   │   ├── get-group-users.md
│   │   ├── update-group-user.md
│   │   └── delete-group-user.md
│   ├── statistic/              # 4 statistics tools
│   │   ├── get-group-statistics.md
│   │   ├── get-member-statistics.md
│   │   ├── get-book-statistics.md
│   │   └── get-doc-statistics.md
│   ├── note/                   # 4 note tools
│   │   ├── list-notes.md
│   │   ├── get-note.md
│   │   ├── create-note.md
│   │   └── update-note.md
│   ├── recycle/                # 3 recycle tools
│   │   ├── list-recycles.md
│   │   ├── restore-recycle.md
│   │   └── destroy-recycle.md
│   ├── upload/                 # 1 upload tool
│   │   └── upload-attachment.md
│   ├── board/                  # 3 board tools
│   │   ├── get-board.md
│   │   ├── create-board.md
│   │   └── update-board.md
│   ├── mine/                   # 2 mine tools
│   │   ├── yuque_get_book_stacks.md
│   │   └── yuque_get_editor_center.md
│   ├── rss/                    # 3 RSS tools
│   │   ├── rss-list-sources.md
│   │   ├── rss-fetch.md
│   │   └── rss-schedule.md
│   ├── crawler/                # 4 crawler tools
│   │   ├── yuque_crawl_fetch.md
│   │   ├── yuque_crawl_extract.md
│   │   ├── yuque_crawl_save.md
│   │   └── crawl-schedule.md
│   └── kv/                     # 4 KV tools
│       ├── yuque_kv_get.md
│       ├── yuque_kv_set.md
│       ├── yuque_kv_delete.md
│       └── yuque_kv_list.md
└── references/
    └── api/                    # API field definitions
        ├── errors.md
        ├── extended_api.md
        ├── doc/
        │   ├── create_doc.md
        │   ├── get_doc.md
        │   ├── update_doc.md
        │   ├── delete_doc.md
        │   ├── list_docs.md
        │   ├── get_doc_versions.md
        │   └── get_doc_version_detail.md
        ├── repo/repo_api.md
        ├── toc/toc_api.md
        ├── search/search_api.md
        ├── user/user_api.md
        ├── group/group_api.md
        ├── statistic/statistic_api.md
        ├── note/note_api.md
        ├── recycle/recycle_api.md
        ├── upload/upload_api.md
        ├── board/board_api.md
        ├── mine/mine_api.md
        ├── rss/rss_api.md
        ├── crawler/crawler_api.md
        └── kv/kv_api.md
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

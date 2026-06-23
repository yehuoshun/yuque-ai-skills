<div align="center">
  <h1>yuque-ai-skills</h1>
  <p><b>62 个使用指导，覆盖语雀 AI MCP 全部工具</b></p>
  <p>
    <a href="https://github.com/yehuoshun/yuque-ai-skills"><img src="https://img.shields.io/badge/指南-62-orange" alt="guides" /></a>
    <a href="https://github.com/yehuoshun/yuque-ai-mcp"><img src="https://img.shields.io/badge/mcp-v2.7.8-blue" alt="mcp" /></a>
  </p>
  <p>
    <a href="README.md">English</a>
  </p>
</div>

---

[yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp) 的 Skill 层——AI Agent 使用指导、场景模式、最佳实践。

> ⚠️ 本仓库不包含可执行代码，只做用法文档。

## 覆盖范围

| 域 | 工具数 | 说明 |
|--------|-------|------|
| doc | 15 | CRUD、版本、Diff、批量、导入导出、跨库复制、资源下载 |
| repo | 8 | CRUD、批量、跨库复制、全量导出 |
| toc | 3 | 获取、更新、批量更新 |
| search | 2 | 通用 + RAG 增强搜索 |
| user | 3 | 用户信息、心跳、团队 |
| group | 3 | 团队成员管理 |
| statistic | 4 | 团队/成员/知识库/文档统计 |
| note | 4 | CRUD + 软删除 |
| recycle | 3 | 列表、恢复、彻底删除 |
| upload | 1 | 文件上传到 CDN |
| board | 3 | 思维导图、流程图、架构图 |
| mine | 2 | 书架列表、编辑中心 |
| rss | 3 | 数据源列表、抓取+去重、定时策略 |
| crawler | 4 | 抓取、CSS 提取、去重写入、定时策略 |
| kv | 4 | 增量分片键值存储 |
| **合计** | **62** | |

## 目录结构

```
skills/           # 15 个域目录，62 个指导文件
references/api/   # API 参考文档（17 个域）
SKILL.md          # 主索引（含端点速查表）
```

## 配套仓库

- [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp) — MCP Server 实现
- 两个仓库保持同步，MCP 新增工具 → Skills 同步新增指导文件

## 许可

MIT
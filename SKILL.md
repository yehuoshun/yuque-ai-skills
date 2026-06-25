# yuque-ai-mcp

语雀全功能 MCP Server，63 个工具 / 15 个域。当用户提到「语雀」「yuque」「知识库」「文档」「团队」等关键词时触发。

## 触发场景

- 语雀知识库管理（创建、删除、列表、复制、导出）
- 文档操作（读写、搜索、导入导出、版本对比）
- 用户/团队信息查询
- 回收站管理
- 目录 TOC 导航
- 画板（思维导图/流程图）
- 统计数据
- 小记
- RSS 抓取
- 网页爬虫
- KV 存储

## 域索引

| 域 | 工具数 | 说明 | 详细 |
|----|--------|------|------|
| doc | 15 | 文档 CRUD、导入导出、版本管理 | [index/doc.md](index/doc.md) |
| repo | 8 | 知识库 CRUD、复制、导出 | [index/repo.md](index/repo.md) |
| search | 3 | 全文搜索、RAG 问答、Cookie 搜索 | [index/search.md](index/search.md) |
| toc | 3 | 目录读取、更新、批量更新 | [index/toc.md](index/toc.md) |
| user | 3 | 用户信息、团队列表、心跳检测 | [index/user.md](index/user.md) |
| group | 3 | 团队成员管理 | [index/group.md](index/group.md) |
| note | 4 | 小记 CRUD | [index/note.md](index/note.md) |
| recycle | 3 | 回收站列表、恢复、销毁 | [index/recycle.md](index/recycle.md) |
| statistic | 4 | 团队/成员/知识库/文档统计 | [index/statistic.md](index/statistic.md) |
| board | 3 | 画板资源 CRUD | [index/board.md](index/board.md) |
| upload | 1 | 上传文件到 CDN | [index/upload.md](index/upload.md) |
| rss | 3 | RSS 源管理、抓取、调度 | [index/rss.md](index/rss.md) |
| crawler | 4 | 网页抓取、提取、去重保存、调度 | [index/crawler.md](index/crawler.md) |
| kv | 4 | KV 存储读写删列 | [index/kv.md](index/kv.md) |
| mine | 2 | 书架分组、编辑中心 | [index/mine.md](index/mine.md) |

## 公共文档

| 文档 | 说明 |
|------|------|
| [common/config.md](common/config.md) | 配置结构 |
| [common/auth.md](common/auth.md) | Token / Cookie 认证 |
| [common/conventions.md](common/conventions.md) | book_id 格式、分页、频率限制 |
| [common/errors.md](common/errors.md) | 统一错误码及处理策略 |

## API 参考

每个域的完整 API 字段定义见 `references/api/`：

| 文件 | 对应域 |
|------|--------|
| `references/api/doc_api.md` | doc |
| `references/api/repo_api.md` | repo |
| `references/api/search_api.md` | search |
| `references/api/toc_api.md` | toc |
| `references/api/user_api.md` | user |
| `references/api/group_api.md` | group |
| `references/api/note_api.md` | note |
| `references/api/recycle_api.md` | recycle |
| `references/api/statistic_api.md` | statistic |
| `references/api/board_api.md` | board |
| `references/api/upload_api.md` | upload |
| `references/api/rss_api.md` | rss |
| `references/api/crawler_api.md` | crawler |
| `references/api/kv_api.md` | kv |
| `references/api/mine_api.md` | mine |
| `references/api/errors.md` | 错误码（同上 common） |
| `references/api/extended_api.md` | 扩展端点 |

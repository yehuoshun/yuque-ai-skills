# yuque-ai-skills

> 语雀 Skill 层 — 工具使用指导、场景模式、最佳实践。不包含可执行代码，只做用法文档。
> **📌 输出裁剪**：所有工具默认返回精简字段（去除 `_serializer`、`type` 等元数据）。传 `raw=true` 可获取全量原始 JSON。

## 触发

当用户提到以下关键词时，加载 MCP 工具集并结合本 Skill 指导执行：

- **操作类**：语雀、yuque、知识库、文档、团队、回收站、导出、导入、小记
- **管理类**：整理语雀、归档、备份、格式化、重命名、合并、分类
- **搜索类**：在语雀搜索、找文档

## API 端点索引

| 端点 | 域 | 说明 | skill 文件 |
|------|-----|------|------------|
| `GET /api/v2/user` | user | 获取当前 Token 的用户详情 | `skills/user/user.md` |
| `GET /api/v2/hello` | user | 心跳检测 | `skills/user/hello.md` |
| `GET /api/v2/users/:id/groups` | user | 获取用户所属的团队列表 | `skills/user/groups.md` |
| `GET /api/v2/search` | search | 通用搜索 | `skills/search/search.md` |
| RAG 检索增强 | search | RAG 搜索 + 读文档 | `skills/search/rag-search.md` |
| `GET /api/v2/groups/:login/users` | group | 获取团队成员列表 | `skills/group/list-users.md` |
| `PUT /api/v2/groups/:login/users/:id` | group | 变更团队成员角色 | `skills/group/update-user.md` |
| `DELETE /api/v2/groups/:login/users/:id` | group | 删除团队成员 | `skills/group/delete-user.md` |
| `GET /api/v2/repos/:book_id/docs` | doc | 获取知识库文档列表 | `skills/doc/list-docs.md` |
| `POST /api/v2/repos/:book_id/docs` | doc | 创建文档 | `skills/doc/create-doc.md` |
| `GET /api/v2/repos/docs/:id` | doc | 获取文档详情 | `skills/doc/get-doc.md` |
| 无（单篇操作） | doc | 导出单篇文档为 Markdown（含图片下载/降级） | `skills/doc/export-doc.md` |
| `PUT /api/v2/repos/:book_id/docs/:id` | doc | 更新文档 | `skills/doc/update-doc.md` |
| `DELETE /api/v2/repos/:book_id/docs/:id` | doc | 删除文档 | `skills/doc/delete-doc.md` |
| `GET /api/v2/doc_versions` | doc | 获取文档历史版本列表 | `skills/doc/versions.md` |
| `GET /api/v2/doc_versions/:id` | doc | 获取文档历史版本详情 | `skills/doc/version-detail.md` |
| `GET /api/v2/repos/:book_id/toc` | toc | 获取知识库目录 | `skills/toc/get-toc.md` |
| `PUT /api/v2/repos/:book_id/toc` | toc | 更新知识库目录 | `skills/toc/update-toc.md` |
| `GET /api/v2/users/:login/repos` | repo | 获取知识库列表 | `skills/repo/list-repos.md` |
| `POST /api/v2/users/:login/repos` | repo | 创建知识库 | `skills/repo/create-repo.md` |
| `GET /api/v2/repos/:book_id` | repo | 获取知识库详情 | `skills/repo/get-repo.md` |
| `PUT /api/v2/repos/:book_id` | repo | 更新知识库 | `skills/repo/update-repo.md` |
| `DELETE /api/v2/repos/:book_id` | repo | 删除知识库 | `skills/repo/delete-repo.md` |
| `GET /api/v2/groups/:login/statistics` | statistic | 团队汇总统计 | `skills/statistic/group-statistics.md` |
| `GET /api/v2/groups/:login/statistics/members` | statistic | 团队成员统计 | `skills/statistic/member-statistics.md` |
| `GET /api/v2/groups/:login/statistics/books` | statistic | 团队知识库统计 | `skills/statistic/book-statistics.md` |
| `GET /api/v2/groups/:login/statistics/docs` | statistic | 团队文档统计 | `skills/statistic/doc-statistics.md` |
| `GET /api/v2/notes` | note | 获取小记列表 | `skills/note/list-notes.md` |
| `GET /api/v2/notes/:id` | note | 获取小记详情 | `skills/note/get-note.md` |
| `POST /api/v2/notes` | note | 创建小记 | `skills/note/create-note.md` |
| `PUT /api/v2/notes/:id` | note | 更新小记 | `skills/note/update-note.md` |
| `GET /api/mine/recycles` | recycle | 列出回收站项目 | `skills/recycle/list-recycles.md` |
| `PUT /api/mine/recycles/:id/restore` | recycle | 恢复回收站项目 | `skills/recycle/restore-recycle.md` |
| `DELETE /api/mine/recycles/:id` | recycle | 彻底删除回收站项目 | `skills/recycle/destroy-recycle.md` |
| `POST /api/upload/attach` | upload | 上传文件到语雀 CDN | `skills/upload/attachment.md` |
| `无（纯工具函数）` | doc | 生成文档嵌入阅读器 URL | `skills/doc/embed-url.md` |
| `MCP: yuque_copy_doc` | doc | 单文档跨库复制（fetch-only 拉取 + Agent 清洗 + 手动创建） | `skills/doc/copy-doc.md` |
| `MCP: yuque_import_url` | doc | 从网页 URL 导入文档（一把梭：抓取+清洗+创建） | `skills/doc/import-url.md` |
| `MCP: yuque_import_file` | doc | 从本地文件导入文档（direct/upload_assets/embed_assets） | `skills/doc/import-file.md` |
| `MCP: yuque_diff_doc_versions` | doc | 对比文档两个版本内容差异（逐行 diff，本地计算） | `skills/doc/diff-doc.md` |
| `MCP: yuque_copy_repo` | repo | 批量跨库复制（Agent 清洗+分类，工具批量创建） | `skills/repo/copy-repo.md` |
| `GET /api/v2/yfm/boards` | board | 获取文档中的画板资源（思维导图/流程图/架构图） | `skills/board/get-board.md` |
| `POST /api/v2/yfm/boards` | board | 在文档中创建画板资源（思维导图/流程图/架构图） | `skills/board/create-board.md` |
| `PUT /api/v2/yfm/boards` | board | 更新文档中的画板资源（思维导图/流程图/架构图） | `skills/board/update-board.md` |
| 批量 GET（并发） | doc | 批量获取文档详情（只读，max 20） | `skills/doc/batch-get-docs.md` |
| 批量 GET（并发） | repo | 批量获取知识库详情（只读，max 20） | `skills/repo/batch-get-repos.md` |
| 无（批量操作） | repo | 批量导出知识库文档为 Markdown（含图片下载/降级） | `skills/repo/export-repo.md` |

## API 使用指南

调用任何 API 前，**必须先读对应的 skill 文件**。完整字段见 `references/api/` 下的对应 API 文档。

**错误码通用**：所有 API 共用错误码，见 `references/api/errors.md`。
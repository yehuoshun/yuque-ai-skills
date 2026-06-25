# doc — 文档操作（15 工具）

> 通用约定见 `common/conventions.md`，错误码见 `common/errors.md`

## 端点速查

| 工具 | 端点 | 说明 |
|------|------|------|
| `yuque_list_docs` | `GET /repos/:book_id/docs` | 获取知识库文档列表 |
| `yuque_create_doc` | `POST /repos/:book_id/docs` | 创建文档 |
| `yuque_get_doc` | `GET /repos/docs/:id` | 获取文档详情 |
| `yuque_update_doc` | `PUT /repos/:book_id/docs/:id` | 更新文档 |
| `yuque_delete_doc` | `DELETE /repos/:book_id/docs/:id` | 删除文档（移入回收站） |
| `yuque_batch_get_docs` | 并发 `GET /repos/docs/:id` | 批量获取文档详情（max 20） |
| `yuque_get_doc_versions` | `GET /doc_versions?doc_id=` | 获取文档历史版本列表 |
| `yuque_get_doc_version_detail` | `GET /doc_versions/:id` | 获取版本详情 |
| `yuque_diff_doc_versions` | 本地 LCS diff | 对比两个版本的行级差异 |
| `yuque_copy_doc` | Agent 清洗 + 创建 | 单文档跨库复制 |
| `yuque_export_doc` | `GET /repos/docs/:id` + 本地导出 | 导出单篇为 Markdown |
| `yuque_export_resources` | 下载 CDN 资源 | 下载文档中的图片/附件 |
| `yuque_import_url` | 抓取 + 清洗 + 创建 | 从网页 URL 导入 |
| `yuque_import_file` | 本地文件 + 创建 | 从本地文件导入 |
| `yuque_embed_url` | 纯工具函数 | 生成嵌入阅读器 URL |

## 详细文档

每个工具的完整参数、返回、注意事项见对应 skill 文件：

| 工具 | Skill 文件 |
|------|-----------|
| `yuque_list_docs` | `skills/doc/list-docs.md` |
| `yuque_create_doc` | `skills/doc/create-doc.md` |
| `yuque_get_doc` | `skills/doc/get-doc.md` |
| `yuque_update_doc` | `skills/doc/update-doc.md` |
| `yuque_delete_doc` | `skills/doc/delete-doc.md` |
| `yuque_batch_get_docs` | `skills/doc/batch-get-docs.md` |
| `yuque_get_doc_versions` | `skills/doc/get-doc-versions.md` |
| `yuque_get_doc_version_detail` | `skills/doc/get-doc-version-detail.md` |
| `yuque_diff_doc_versions` | `skills/doc/diff-doc-versions.md` |
| `yuque_copy_doc` | `skills/doc/copy-doc.md` |
| `yuque_export_doc` | `skills/doc/export-doc.md` |
| `yuque_export_resources` | `skills/doc/export-resources.md` |
| `yuque_import_url` | `skills/doc/import-url.md` |
| `yuque_import_file` | `skills/doc/import-file.md` |
| `yuque_embed_url` | `skills/doc/embed-url.md` |

## API 参考

完整 API 字段定义见 `references/api/doc_api.md`

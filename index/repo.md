# repo — 知识库管理（8 工具）

> 通用约定见 `common/conventions.md`，错误码见 `common/errors.md`

## 端点速查

| 工具 | 端点 | 说明 |
|------|------|------|
| `yuque_list_repos` | `GET /users/:login/repos` / `GET /groups/:login/repos` | 获取知识库列表 |
| `yuque_create_repo` | `POST /users/:login/repos` / `POST /groups/:login/repos` | 创建知识库 |
| `yuque_get_repo` | `GET /repos/:book_id` | 获取知识库详情（含 toc_yml） |
| `yuque_update_repo` | `PUT /repos/:book_id` | 更新知识库 |
| `yuque_delete_repo` | `DELETE /repos/:book_id` | 删除知识库 |
| `yuque_batch_get_repos` | 并发 `GET /repos/:book_id` | 批量获取知识库详情（max 20） |
| `yuque_copy_repo` | LLM 分类 + 批量创建 | 批量跨库复制 |
| `yuque_export_repo` | 批量导出 | 导出整个知识库为 Markdown |

## 详细文档

| 工具 | Skill 文件 |
|------|-----------|
| `yuque_list_repos` | `skills/repo/list-repos.md` |
| `yuque_create_repo` | `skills/repo/create-repo.md` |
| `yuque_get_repo` | `skills/repo/get-repo.md` |
| `yuque_update_repo` | `skills/repo/update-repo.md` |
| `yuque_delete_repo` | `skills/repo/delete-repo.md` |
| `yuque_batch_get_repos` | `skills/repo/batch-get-repos.md` |
| `yuque_copy_repo` | `skills/repo/copy-repo.md` |
| `yuque_export_repo` | `skills/repo/export-repo.md` |

## API 参考

完整 API 字段定义见 `references/api/repo_api.md`

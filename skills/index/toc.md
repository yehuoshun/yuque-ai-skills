# toc — 目录操作（3 工具）

> 通用约定见 `skills/common/conventions.md`，错误码见 `skills/common/errors.md`

## 端点速查

| 工具 | 端点 | 说明 |
|------|------|------|
| `yuque_get_toc` | `GET /repos/:book_id/toc` | 获取知识库目录 |
| `yuque_update_toc` | `PUT /repos/:book_id/toc` | 更新知识库目录 |
| `yuque_batch_update_toc` | 批量 PUT | 批量更新目录（Agent 出计划，Tool 执行） |

## 详细文档

| 工具 | Skill 文件 |
|------|-----------|
| `yuque_get_toc` | `skills/toc/get-toc.md` |
| `yuque_update_toc` | `skills/toc/update-toc.md` |
| `yuque_batch_update_toc` | `skills/toc/batch-update-toc.md` |

## API 参考

完整 API 字段定义见 `references/api/toc_api.md`

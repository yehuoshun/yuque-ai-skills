# recycle — 回收站（3 工具）

> 需要 Cookie 认证，见 `skills/common/auth.md`。通用约定见 `skills/common/conventions.md`，错误码见 `skills/common/errors.md`

## 端点速查

| 工具 | 端点 | 说明 |
|------|------|------|
| `yuque_list_recycles` | `GET /api/recycles` (Cookie) | 列出回收站项目 |
| `yuque_restore_recycle` | `PUT /api/recycles/:id` (Cookie) | 恢复回收站项目 |
| `yuque_destroy_recycle` | `DELETE /api/recycles/:id` (Cookie) | 彻底删除回收站项目 |

## 详细文档

| 工具 | Skill 文件 |
|------|-----------|
| `yuque_list_recycles` | `skills/recycle/list-recycles.md` |
| `yuque_restore_recycle` | `skills/recycle/restore-recycle.md` |
| `yuque_destroy_recycle` | `skills/recycle/destroy-recycle.md` |

## API 参考

完整 API 字段定义见 `references/api/recycle_api.md`

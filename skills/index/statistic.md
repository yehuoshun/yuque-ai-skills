# statistic — 统计（4 工具）

> 通用约定见 `skills/common/conventions.md`，错误码见 `skills/common/errors.md`

## 端点速查

| 工具 | 端点 | 说明 |
|------|------|------|
| `yuque_get_group_statistics` | `GET /groups/:login/statistics` | 获取团队汇总统计数据 |
| `yuque_get_member_statistics` | `GET /groups/:login/statistics/members` | 获取团队成员统计数据 |
| `yuque_get_book_statistics` | `GET /groups/:login/statistics/books` | 获取团队知识库统计数据 |
| `yuque_get_doc_statistics` | `GET /groups/:login/statistics/docs` | 获取团队文档统计数据 |

## 详细文档

| 工具 | Skill 文件 |
|------|-----------|
| `yuque_get_group_statistics` | `skills/statistic/get-group-statistics.md` |
| `yuque_get_member_statistics` | `skills/statistic/get-member-statistics.md` |
| `yuque_get_book_statistics` | `skills/statistic/get-book-statistics.md` |
| `yuque_get_doc_statistics` | `skills/statistic/get-doc-statistics.md` |

## API 参考

完整 API 字段定义见 `references/api/statistic_api.md`

# board — 画板（3 工具）

> 通用约定见 `common/conventions.md`，错误码见 `common/errors.md`

## 端点速查

| 工具 | 端点 | 说明 |
|------|------|------|
| `yuque_get_board` | `GET /repos/:book_id/docs/:id/board` | 获取文档中的画板资源 |
| `yuque_create_board` | `POST /repos/:book_id/docs/:id/board` | 在文档中创建画板资源 |
| `yuque_update_board` | `PUT /repos/:book_id/docs/:id/board` | 更新文档中的画板资源 |

## 详细文档

| 工具 | Skill 文件 |
|------|-----------|
| `yuque_get_board` | `skills/board/get-board.md` |
| `yuque_create_board` | `skills/board/create-board.md` |
| `yuque_update_board` | `skills/board/update-board.md` |

## API 参考

完整 API 字段定义见 `references/api/board_api.md`

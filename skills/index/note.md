# note — 小记（4 工具）

> 通用约定见 `skills/common/conventions.md`，错误码见 `skills/common/errors.md`

## 端点速查

| 工具 | 端点 | 说明 |
|------|------|------|
| `yuque_list_notes` | `GET /users/:login/notes` | 获取小记列表 |
| `yuque_get_note` | `GET /users/:login/notes/:id` | 获取小记详情 |
| `yuque_create_note` | `POST /users/:login/notes` | 创建小记 |
| `yuque_update_note` | `PUT /users/:login/notes/:id` | 更新小记 |

## 详细文档

| 工具 | Skill 文件 |
|------|-----------|
| `yuque_list_notes` | `skills/note/list-notes.md` |
| `yuque_get_note` | `skills/note/get-note.md` |
| `yuque_create_note` | `skills/note/create-note.md` |
| `yuque_update_note` | `skills/note/update-note.md` |

## API 参考

完整 API 字段定义见 `references/api/note_api.md`

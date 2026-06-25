# group — 团队（3 工具）

> 通用约定见 `common/conventions.md`，错误码见 `common/errors.md`

## 端点速查

| 工具 | 端点 | 说明 |
|------|------|------|
| `yuque_get_group_users` | `GET /groups/:login/users` | 获取团队成员列表 |
| `yuque_update_group_user` | `PUT /groups/:login/users/:user_id` | 变更团队成员角色 |
| `yuque_delete_group_user` | `DELETE /groups/:login/users/:user_id` | 删除团队成员 |

## 详细文档

| 工具 | Skill 文件 |
|------|-----------|
| `yuque_get_group_users` | `skills/group/get-group-users.md` |
| `yuque_update_group_user` | `skills/group/update-group-user.md` |
| `yuque_delete_group_user` | `skills/group/delete-group-user.md` |

## API 参考

完整 API 字段定义见 `references/api/group_api.md`

# user — 用户（3 工具）

> 通用约定见 `common/conventions.md`，错误码见 `common/errors.md`

## 端点速查

| 工具 | 端点 | 说明 |
|------|------|------|
| `yuque_hello` | `GET /user` | 心跳检测，验证 Token 有效性 |
| `yuque_get_user` | `GET /user` | 获取当前 Token 的用户详情 |
| `yuque_get_user_groups` | `GET /user/groups` | 获取用户所属的团队列表 |

## 详细文档

| 工具 | Skill 文件 |
|------|-----------|
| `yuque_hello` | `skills/user/hello.md` |
| `yuque_get_user` | `skills/user/get-user.md` |
| `yuque_get_user_groups` | `skills/user/get-user-groups.md` |

## API 参考

完整 API 字段定义见 `references/api/user_api.md`

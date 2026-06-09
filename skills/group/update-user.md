# yuque_update_group_user

## API

`PUT /api/v2/groups/{login}/users/{id}` — 完整文档见 `references/api/group_api.md` → 变更成员。

## 什么时候调

- 调整团队成员角色（升管理员 / 降成员 / 设为只读）
- 用户说「把 XX 设为管理员」「把 XX 降为只读」

## 调用

```bash
curl -s -X PUT -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"role": 0}' \
  "https://www.yuque.com/api/v2/groups/{login}/users/{id}"
```

## 参数要点

| 参数 | 位置 | 类型 | 说明 |
|------|------|------|------|
| `login` | path | string | 团队 Login 或 ID（必填） |
| `id` | path | string | 用户 Login 或 ID（必填） |
| `role` | body | int | 0=管理员 / 1=成员 / 2=只读成员（默认 1） |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `role` | 变更后的角色 |
| `user.login` | 被变更用户 login |
| `user.name` | 被变更用户昵称 |
| `group.login` | 团队路径 |

完整字段见 `references/api/group_api.md`。

## 调完干嘛

- 告知用户变更结果（「已把 {name} 设为 {角色}」）
- 若需确认变更后状态，调 `yuque_get_group_users` 对比

## 别干的事

- 别把自己降级为成员（降完可能没法改回去）
- 别在不确认当前角色的情况下直接覆盖
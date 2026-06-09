# yuque_delete_group_user

## API

`DELETE /api/v2/groups/{login}/users/{id}` — 完整文档见 `references/api/group_api.md` → 删除成员。

## 什么时候调

- 将成员移出团队
- 用户说「把 XX 踢出团队」「移除 XX」

## 调用

```bash
curl -s -X DELETE -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/groups/{login}/users/{id}"
```

## 参数要点

| 参数 | 位置 | 类型 | 说明 |
|------|------|------|------|
| `login` | path | string | 团队 Login 或 ID（必填） |
| `id` | path | string | 用户 Login 或 ID（必填） |

## 返回

```json
{
  "data": {
    "user_id": "string"
  }
}
```

## 调完干嘛

- 告知用户移除结果
- 确认是否需要再调 `yuque_get_group_users` 验证

## 别干的事

- 别删自己——删完就没法操作了
- 别在不确认用户身份的情况下直接删
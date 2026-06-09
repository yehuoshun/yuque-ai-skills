# yuque_get_user_groups

## API

`GET /api/v2/users/{id}/groups` — 完整文档见 `references/api/user_api.md` → 获取用户的团队。

## 什么时候调

- 获取当前用户所属的团队
- 需要 team login 去操作团队知识库时

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/users/{id}/groups?offset=0&limit=100"
```

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `id` | 团队 ID |
| `login` | 团队 login，后续操作团队知识库需要 |
| `name` | 团队名称 |

## 调完干嘛

- 拿到 `login` 记住，后续团队操作（增删改知识库）用它构造 namespace
- 无团队可操作时提示用户

## 别干的事

- 别默认用户一定有团队——可能没有
- 参数 `id` 是用户 ID 不是 user login
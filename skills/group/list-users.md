# yuque_get_group_users

## API

`GET /api/v2/groups/{login}/users` — 完整文档见 `references/api/group_api.md`。

## 什么时候调

- 查看团队成员列表
- 需要知道团队里有哪些人、各自什么角色
- 确认某用户是否在团队中

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/groups/{login}/users?offset=0&role=0"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `login` | 必填，团队 Login 或 ID（从 `yuque_get_user_groups` 返回的 `login` 字段获取） |
| `role` | 0=管理员 / 1=成员 / 2=只读成员，不传返回全部 |
| `offset` | 分页偏移，默认 0，每页固定 100 条 |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `user_id` | 成员 ID |
| `role` | 0=管理员 / 1=成员 / 2=只读成员 |
| `user.login` | 用户 login |
| `user.name` | 用户昵称 |
| `user.avatar_url` | 头像 |
| `group.login` | 团队路径 |
| `group.name` | 团队名称 |
| `group.members_count` | 团队总人数 |

完整字段见 `references/api/group_api.md`。

## 调完干嘛

- 展示成员列表（昵称 + 角色 + 头像）
- 团队登录名可从返回的 `group.login` 直接获取，无需再调 `yuque_get_user_groups`

## 别干的事

- 别用 `user_id` 当 `login` 传参——端点支持 login 或 ID 两种传法
- 别期望能创建/更新/删除团队——语雀 API 不开放这些操作
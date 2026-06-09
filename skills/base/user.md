# yuque_get_user

获取当前语雀用户信息。

## 触发

- 用户问「我是谁」「我的语雀账号」「查看用户信息」
- 需要确认当前 Token 对应的用户身份时

## 用法

直接调用 `yuque_get_user`，无需参数。

## 返回

```json
{
  "id": 25689388,
  "login": "yehuoshun",
  "name": "夜灬瞬",
  "avatar_url": "https://cdn.nlark.com/yuque/0/2021/png/25689388/...",
  "description": "..."
}
```

## 注意

- 返回的 `id` 和 `login` 是后续操作（如指定知识库归属）的关键参数
- Token 决定了返回哪个用户，无法查询其他用户
# yuque_get_user

## API

`GET /api/v2/user` — 完整文档见 `references/api_reference.md` → 用户 API → 获取当前用户。

## 什么时候调

- 任何语雀操作前，如果不知道当前用户身份 → 先调这个
- 用户问「我的语雀账号」「我是谁」「Token 是谁的」

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" "https://www.yuque.com/api/v2/user"
```

或用 MCP 工具 `yuque_get_user`。

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `id` | 用户 ID，创建知识库时需要 |
| `login` | 构造 namespace（`{login}/{book_slug}`） |
| `name` | 展示给用户（「当前是 {name}」） |

完整字段见 `references/api_reference.md` 用户 API 章节。

## 调完干嘛

1. 记住 `login`，后续所有操作都基于它
2. 只告诉用户「当前是 {name}（{login}）」，不 dump 完整 JSON

## 别干的事

- 别当健康检查用
- 别每次操作前重复调——一次记住就行
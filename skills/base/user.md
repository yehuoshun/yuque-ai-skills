# yuque_get_user

## 什么时候调

- 任何语雀操作前，如果不知道当前用户身份 → 先调这个
- 用户问「我的语雀账号」「我是谁」「Token 是谁的」
- 需要拿 `login` 去构造知识库 namespace（`{login}/{book_slug}`）时

## 调完干嘛

拿到 `id` 和 `login` 后：

| 字段 | 后续用途 |
|------|---------|
| `id` | 创建知识库时的 `user_id`、文档操作鉴权 |
| `login` | 构造 namespace（如 `yehuoshun/xxx`）、创建知识库路径 |

**不需要**展示完整返回给用户。只告诉用户「当前是 {name}（{login}）」。

## 别干的事

- 别当作健康检查来用（这是用户接口，不是 health check）
- 别在每次操作前都调一次——调一次记住 `login` 就行
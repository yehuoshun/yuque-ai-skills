# yuque_batch_get_repos — 批量获取知识库详情

## 端点

并发 `GET /api/v2/repos/:book_id`（无原生批量端点，MCP 层 `Promise.all` 实现）

## 用途

一次请求获取多个知识库的详情（名称、描述、公开性、文档数、点赞数等），避免串行调用。

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `ids` | string | ✅ | 知识库 ID 的 JSON 数组，如 `[123, 456]` 或 `["group/repo-a", "group/repo-b"]`，最多 20 个 |

## 返回

```json
{
  "total": 3,
  "ok": 2,
  "repos": {
    "123": { "id": 123, "name": "...", "description": "...", "items_count": 42 },
    "456": { "id": 456, "name": "...", "description": "...", "items_count": 18 }
  },
  "errors": [
    { "id": 789, "detail": { ... } }
  ]
}
```

- `total`: 请求的知识库总数
- `ok`: 成功获取的数量
- `repos`: 按 id 分组的知识库详情
- `errors`: 获取失败的知识库及错误详情

## 约束

- **只读操作**，不修改任何数据
- 最多 20 个知识库（超过返回错误）
- 并发请求，不保证返回顺序（但结果按 id 分组）

## 示例

```json
{
  "ids": "[\"yehuoshun/STS2-ShunMod\", \"yehuoshun/yuque-ai-skills\"]"
}
```

## 注意事项

- 语雀 API 有频率限制（默认 1000 次/小时），批量操作会消耗对应数量的配额
- 大量知识库建议分页调用，每次 ≤ 20 个
- 支持 namespace 格式（`group/repo_slug`）和纯数字 ID
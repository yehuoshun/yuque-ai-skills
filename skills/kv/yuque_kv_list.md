# yuque_kv_list

## 功能

列出 KV 知识库中所有命名空间（文档）。

## 使用场景

- 查看有哪些 KV 命名空间
- 检查某个主题是否已有 KV 存储
- 运维排查

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `repo` | string | ❌ | KV 知识库 ID 或 namespace，默认使用 config.json 中 kv.default_repo |
| `raw` | boolean | ❌ | 返回原始 JSON（默认 false） |

## 调用示例

```json
{}
```

## 返回示例

```json
{
  "repo": "80197550",
  "count": 3,
  "namespaces": [
    { "namespace": "cnblogs", "title": "cnblogs", "updated_at": "2026-06-16T14:00:00.000Z" },
    { "namespace": "weibo", "title": "weibo", "updated_at": "2026-06-15T10:00:00.000Z" },
    { "namespace": "test", "title": "test", "updated_at": "2026-06-16T14:05:00.000Z" }
  ]
}
```

## 注意事项

- 列出的是 KV 知识库中所有文档，每个文档的 slug 就是 namespace
- 不解析文档内容，只返回基本元信息
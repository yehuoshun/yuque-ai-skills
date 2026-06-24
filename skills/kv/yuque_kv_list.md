# yuque_kv_list

## 功能

列出 config.json 中所有已配置的 KV 命名空间。

## 使用场景

- 查看有哪些 KV 命名空间
- 检查某个主题是否已有 KV 存储
- 运维排查

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `raw` | boolean | ❌ | 返回原始 JSON（默认 false） |

## 调用示例

```json
{}
```

## 返回示例

```json
{
  "count": 2,
  "namespaces": [
    {
      "namespace": "cnblogs",
      "book_id": 0,
      "shards": 2,
      "doc_ids": [0, 274164065]
    },
    {
      "namespace": "weibo",
      "book_id": 0,
      "shards": 1,
      "doc_ids": [274164066]
    }
  ]
}
```

## 注意事项

- 直接从 config.json 读取，不调 API
- 不解析文档内容，只返回配置元信息
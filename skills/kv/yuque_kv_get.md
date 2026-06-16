# yuque_kv_get

## 功能

读取一个 KV 命名空间的完整 JSON key-value map。从 config.json 的 kv.namespaces 获取分片信息，按 docs 数组逐个读取合并。

## 使用场景

- 去重检查：查某个 key 是否已存在
- 配置读取：加载命名空间下的所有配置
- 状态查询：查看某个主题的 KV 存储全貌

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `namespace` | string | ✅ | KV 命名空间，如 `cnblogs`、`weibo`。需先在 config.json 中配置。 |
| `raw` | boolean | ❌ | 返回原始 JSON（默认 false） |

## 调用示例

```json
{
  "namespace": "cnblogs"
}
```

## 返回示例

```json
{
  "namespace": "cnblogs",
  "book_id": 80197550,
  "shards": 2,
  "count": 5000,
  "data": {
    "cnblogs-123456": "https://www.cnblogs.com/xxx/p/123456.html",
    "cnblogs-789012": "https://www.cnblogs.com/yyy/p/789012.html"
  }
}
```

## 注意事项

- namespace 需先在 config.json 中配置 book_id 和 docs 数组
- 自动合并所有分片（docs 数组顺序）返回完整 map
- 返回的 `data` 是整个 map 的完整内容，数据量大时注意 token 消耗

## 分片存储

```json
{
  "kv": {
    "enabled": true,
    "namespaces": {
      "cnblogs": {
        "book_id": 80197550,
        "docs": [274164064, 274164065]
      }
    }
  }
}
```

- `book_id`：该 namespace 使用的语雀知识库 ID
- `docs`：分片文档的 doc_id 数组，按顺序读取合并
# yuque_kv_set

## 功能

增量设置 KV 命名空间中的一个 key-value 对。只读最后一个分片，判断大小后 PUT 更新或 POST 创建新分片。

## 使用场景

- 去重标记：爬取/抓取后标记已处理
- 配置更新：修改某个 key 的值
- 状态记录：记录处理进度、时间戳等

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `namespace` | string | ✅ | KV 命名空间，如 `cnblogs`、`weibo`。需先在 config.json 中配置。 |
| `key` | string | ✅ | 要设置的 key |
| `value` | string | ✅ | 要存储的值 |
| `raw` | boolean | ❌ | 返回原始 JSON（默认 false） |

## 调用示例

```json
{
  "namespace": "cnblogs",
  "key": "cnblogs-123456",
  "value": "https://www.cnblogs.com/xxx/p/123456.html"
}
```

## 返回示例

```json
{
  "namespace": "cnblogs",
  "key": "cnblogs-123456",
  "value": "https://www.cnblogs.com/xxx/p/123456.html",
  "shards": 2
}
```

## 注意事项

- 增量写入：只读最后一个分片，不读全量
- 单分片 body 上限 250KB，超出自动创建新分片
- 新分片的 doc_id 自动追加到 config.json 的 docs 数组
- namespace 需先在 config.json 中配置 book_id

## 分片配置

```json
{
  "kv": {
    "enabled": true,
    "namespaces": {
      "cnblogs": {
        "book_id": 80197550,
        "docs": [274164064]
      }
    }
  }
}
```
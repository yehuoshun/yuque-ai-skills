# yuque_kv_delete

## 功能

从 KV 命名空间中删除一个 key。遍历分片查找 key，找到后 PUT 更新该分片。

## 使用场景

- 清理过期条目
- 重置去重状态
- 删除误标记的 key

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `namespace` | string | ✅ | KV 命名空间 |
| `key` | string | ✅ | 要删除的 key |
| `raw` | boolean | ❌ | 返回原始 JSON（默认 false） |

## 调用示例

```json
{
  "namespace": "cnblogs",
  "key": "cnblogs-123456"
}
```

## 返回示例

```json
{
  "namespace": "cnblogs",
  "key": "cnblogs-123456",
  "action": "deleted",
  "shards": 2
}
```

## 注意事项

- 如果 key 不存在，返回 `action: "not_found"`，不报错
- 遍历所有分片查找 key，找到后只更新该分片
- 不会删除空分片文档（即使该分片变空）
# yuque_kv_delete

## 功能

从 KV 命名空间中删除一个 key。

## 使用场景

- 清理过期条目
- 重置去重状态
- 删除误标记的 key

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `namespace` | string | ✅ | KV 命名空间 |
| `key` | string | ✅ | 要删除的 key |
| `repo` | string | ❌ | KV 知识库 ID 或 namespace，默认使用 config.json 中 kv.default_repo |
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
  "total_keys": 42
}
```

## 注意事项

- 如果 key 不存在，返回 `action: "not_found"`，不报错
- 删除后文档会自动更新，空 map 的文档仍保留（不会删除文档本身）
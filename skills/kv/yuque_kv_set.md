# yuque_kv_set

## 功能

在 KV 命名空间中设置一个 key-value 对。读取已有 map → 更新/新增 key → 写回。

## 使用场景

- 去重标记：爬取/抓取后标记已处理
- 配置更新：修改某个 key 的值
- 状态记录：记录处理进度、时间戳等

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `namespace` | string | ✅ | KV 命名空间，如 `cnblogs`、`weibo` |
| `key` | string | ✅ | 要设置的 key |
| `value` | string | ✅ | 要存储的值 |
| `repo` | string | ❌ | KV 知识库 ID 或 namespace，默认使用 config.json 中 kv.default_repo |
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
  "action": "created",
  "total_keys": 43
}
```

## 注意事项

- 如果 key 已存在，action 为 `updated`；否则为 `created`
- 每次 set 涉及 2-3 次 API 调用（读 + 写），批量操作建议攒一批后一次性调用
- 底层存储：单文档 JSON map，一个 namespace 一篇文档
# yuque_kv_get

## 功能

读取一个 KV 命名空间的完整 JSON key-value map。

## 使用场景

- 去重检查：查某个 key 是否已存在
- 配置读取：加载命名空间下的所有配置
- 状态查询：查看某个主题的 KV 存储全貌

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `namespace` | string | ✅ | KV 命名空间，如 `cnblogs`、`weibo` |
| `repo` | string | ❌ | KV 知识库 ID 或 namespace，默认使用 config.json 中 kv.default_repo |
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
  "repo": "80197550",
  "count": 42,
  "data": {
    "cnblogs-123456": "https://www.cnblogs.com/xxx/p/123456.html",
    "cnblogs-789012": "https://www.cnblogs.com/yyy/p/789012.html"
  }
}
```

## 注意事项

- 一个 namespace 对应语雀知识库里的一篇文档（slug = namespace）
- 文档 body 是 JSON 格式的 `{key: value}` map
- 首次调用时如果 namespace 不存在，返回空 map `{}`
- 返回的 `data` 是整个 map 的完整内容，数据量大时注意 token 消耗
# kv — KV 存储（4 工具）

> 通用约定见 `common/conventions.md`，错误码见 `common/errors.md`

## 端点速查

| 工具 | 端点 | 说明 |
|------|------|------|
| `yuque_kv_get` | 读取 + 分片合并 | 读取 KV 命名空间的完整 JSON map |
| `yuque_kv_set` | 增量写入 + 自动分片 | 增量设置 key-value，超 250KB 自动分片 |
| `yuque_kv_delete` | 遍历分片查找删除 | 遍历分片查找并删除 key |
| `yuque_kv_list` | 配置读取 | 列出已配置的 KV 命名空间 |

## 详细文档

| 工具 | Skill 文件 |
|------|-----------|
| `yuque_kv_get` | `skills/kv/yuque_kv_get.md` |
| `yuque_kv_set` | `skills/kv/yuque_kv_set.md` |
| `yuque_kv_delete` | `skills/kv/yuque_kv_delete.md` |
| `yuque_kv_list` | `skills/kv/yuque_kv_list.md` |

## API 参考

完整 API 字段定义见 `references/api/kv_api.md`

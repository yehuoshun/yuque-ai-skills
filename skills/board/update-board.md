# yuque_update_board

> 端点：`PUT /api/v2/yfm/boards`
> 说明：更新文档中的画板资源（思维导图/流程图/架构图）

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `doc_id` | number | 二选一 | 文档 ID |
| `url` | string | 二选一 | 文档 URL |
| `resource_id` | string | **必填** | 画板资源 ID（从 `board://<id>` 中提取） |
| `text` | string | 二选一 | 新的文本 DSL 内容 |
| `dsl` | string | 二选一 | 新的 JSON DSL 对象（JSON 字符串） |

## 响应

返回更新后的画板资源完整信息。

## 使用场景

- 修改文档中已有的思维导图结构
- 更新流程图的节点和连线
- 调整架构图的组件布局

## 注意事项

- `doc_id` 和 `url` 必须二选一
- `text` 和 `dsl` 必须二选一：`text` 更新文本 DSL，`dsl` 更新 JSON DSL
- `resource_id` 从 `board://<id>` 中提取，只传 ID 部分
- `dsl` 参数传 JSON 字符串，工具会自动 parse
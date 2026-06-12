# yuque_get_resource

> 端点：`GET /api/v2/yfm/boards`
> 说明：获取文档中的画板资源（思维导图/流程图/架构图），返回 JSON DSL 和摘要统计

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `doc_id` | number | 二选一 | 文档 ID |
| `url` | string | 二选一 | 文档 URL |
| `resource_id` | string | **必填** | 画板资源 ID（从 `board://<id>` 中提取，只传 ID） |

## 响应

```json
{
  "doc_id": 123,
  "title": "架构设计",
  "url": "https://...",
  "board": {
    "dsl": { ... },
    "summary": {
      "cell_count": 42,
      "type_counts": {},
      "shape_counts": {}
    }
  }
}
```

## 使用场景

- 读取文档中已有的思维导图数据和结构
- 获取流程图的节点和连线信息
- 分析架构图的组件和关系

## 注意事项

- `resource_id` 从语雀文档中的 `board://<resource_id>` 链接提取，只传 ID 部分，不要传 `board://` 前缀
- `doc_id` 和 `url` 必须二选一
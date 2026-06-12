# board — 画板资源 API

语雀文档中的结构化画板资源（Board），支持思维导图、流程图、架构图三种类型。

## 端点

| 端点 | 方法 | 说明 |
|------|------|------|
| `/yfm/boards` | GET | 获取画板资源 |
| `/yfm/boards` | POST | 创建画板资源 |
| `/yfm/boards` | PUT | 更新画板资源 |

## 画板类型

| 类型 | 值 | 说明 |
|------|-----|------|
| 思维导图 | `mindmap` | 树形结构，知识整理 |
| 流程图 | `flowchart` | 节点连线，流程建模 |
| 架构图 | `architecturediagram` | 系统架构可视化 |

### yuque_get_board

```
GET /api/v2/yfm/boards?resource_type=board&src={resource_id}&doc_id={id}
```

获取文档中已有的画板资源 JSON DSL 和摘要统计。

**参数**：`doc_id` 或 `url`（二选一）、`resource_id`（必填，从 `board://<id>` 中提取）

### yuque_create_board

```
POST /api/v2/yfm/boards
```

在文档中插入新的画板资源。

**参数**：`doc_id` 或 `url`（二选一）、`type`（必填，mindmap/flowchart/architecturediagram）、`dsl`（必填，文本 DSL 内容）、`insert_after_lake_id`（可选）

### yuque_update_board

```
PUT /api/v2/yfm/boards
```

修改文档中已有的画板资源。

**参数**：`doc_id` 或 `url`（二选一）、`resource_id`（必填）、`text` 或 `dsl`（二选一）

## 注意事项

- `resource_id` 从语雀文档中的 `board://<resource_id>` 链接提取，只传 ID 部分
- 创建和更新时需要提供合法的 DSL 内容，格式取决于画板类型
- 这是语雀编辑器内嵌卡片功能，非 AI 画图
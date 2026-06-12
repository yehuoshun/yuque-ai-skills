# yuque_create_resource

> 端点：`POST /api/v2/yfm/boards`
> 说明：在文档中创建画板资源（思维导图/流程图/架构图）

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `doc_id` | number | 二选一 | 文档 ID |
| `url` | string | 二选一 | 文档 URL |
| `type` | string | **必填** | 画板类型：`mindmap` / `flowchart` / `architecturediagram` |
| `dsl` | string | **必填** | 画板文本 DSL 内容 |
| `insert_after_lake_id` | string | 可选 | 插入到指定 Lake 节点之后，不填则追加到文档末尾 |

## 画板类型

| 值 | 说明 |
|-----|------|
| `mindmap` | 思维导图 |
| `flowchart` | 流程图 |
| `architecturediagram` | 架构图 |

## 响应

返回创建的画板资源完整信息，包含 `board.dsl` 和 `board.summary`。

## 使用场景

- 程序化在文档中插入思维导图
- 批量生成流程图到文档
- 自动化文档中的架构图更新

## 注意事项

- `doc_id` 和 `url` 必须二选一
- DSL 内容格式取决于 `type`，需要合法的画板 DSL 语法
- 这是操作语雀编辑器内嵌卡片，不是 AI 画图
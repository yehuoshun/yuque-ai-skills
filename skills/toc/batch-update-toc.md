# 批量更新知识库目录

## API

`PUT /api/v2/repos/{book_id}/toc`（批量）— Agent 出变更计划（ops），Tool 原样执行，零决策。纯 TOC 操作。

> ⚠️ 跨库复制请用 `yuque_copy_doc`，本工具只做 TOC 增删改。

## 什么时候调

- Agent 分析完 TOC 结构后，批量执行目录变更
- 本库整理：创建目录、移动节点、挂载文档

## 调用

```bash
mcporter call "yuque-mcp.yuque_batch_update_toc" --args '{
  "book_id": "79841574",
  "confirm": "RESTRUCTURE",
  "ops": "[{\"action\":\"createTitle\",\"title\":\"设计模式\"},{\"action\":\"moveNode\",\"node_uuid\":\"xxx\",\"target_title\":\"设计模式\"}]"
}'
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `book_id` | 必填，知识库 ID 或 namespace |
| `ops` | 必填，JSON 数组，Agent 生成的变更计划 |
| `confirm` | 必填 `"RESTRUCTURE"` |

## ops 操作类型

| action | 说明 | 必填字段 | 可选字段 |
|--------|------|---------|----------|
| `createTitle` | 创建目录节点（自动去重） | `title` | `target_uuid` |
| `appendNode` | 挂载文档到目录 | `doc_ids`（JSON 数组字符串） | `target_uuid` / `target_title` |
| `removeNode` | 删除目录节点 | `node_uuid` | — |
| `moveNode` | 移动节点 | `node_uuid` | `target_uuid` / `target_title` / `doc_id` |

### createTitle 自动去重

创建前先查缓存，目录已存在则复用 uuid 不重复创建，返回 `"复用已有目录: xxx"`。

### appendNode 挂载逻辑

- **无 `target_uuid`**：直接挂到根目录
- **有 `target_uuid`**：先挂根再移。语雀 API 的 `target_uuid` 对游离文档不可靠，工具自动处理三步：挂根 → 删根 → 挂目标

### moveNode 游离文档处理

- 节点在 TOC 中：正常 remove + append
- 节点不在 TOC 中（游离文档）：提供 `doc_id` 字段，工具自动挂根再移
- 不提供 `doc_id` 且不在 TOC：返回明确错误提示

### target_title 按名称查找

`appendNode` 和 `moveNode` 支持 `target_title`，按 TITLE 名称自动查找目标节点 uuid，Agent 不用先查。

### TOC 缓存

每个知识库 TOC 缓存 24h，写操作后自动失效，每 1h 清理过期条目。

## 返回

```json
{
  "book_id": "79841574",
  "total": 4,
  "success": 4,
  "failed": 0,
  "results": [
    {"index": 0, "action": "createTitle", "success": true, "detail": "复用已有目录: 设计模式", "new_node_uuid": "xxx"}
  ]
}
```

## 跨库整理流程

```
1. yuque_copy_doc  → 复制文档到目标库（doc 域）
2. batch_update_toc → 创建目录 + 挂载文档（toc 域）
```

## 别干的事

- 别用本工具跨库复制文档（用 `yuque_copy_doc`）
- 别忘记 `confirm` 必须传 `"RESTRUCTURE"`
- 移动文档用 `moveNode`，别用 `appendNode`（appendNode 不会改变已有文档的 parent_uuid）
- 游离文档（不在 TOC 中）用 `moveNode` + `doc_id`，或 `appendNode` + `target_uuid`（工具自动挂根再移）
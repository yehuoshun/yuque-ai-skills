# 批量更新知识库目录

## API

`PUT /api/v2/repos/{book_id}/toc`（批量）— Agent 出变更计划（ops），Tool 原样执行，零决策。支持本库整理和跨库整理。

## 什么时候调

- Agent 分析完 TOC 结构后，批量执行目录变更
- 跨库整理：从源库复制文档到目标库
- 本库整理：创建目录、移动节点、挂载文档

## 调用

```bash
mcporter call "yuque-mcp.yuque_batch_update_toc" --args '{
  "book_id": "79841574",
  "target_book_id": "80346623",
  "mode": "copy",
  "confirm": "RESTRUCTURE",
  "ops": "[{\"action\":\"createTitle\",\"title\":\"设计模式\"},{\"action\":\"copyDoc\",\"doc_id\":272879798}]"
}'
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `book_id` | 必填，源知识库 ID 或 namespace |
| `target_book_id` | 目标库（不传=本库整理） |
| `ops` | 必填，JSON 数组，Agent 生成的变更计划 |
| `mode` | `copy`（默认，保留源）/ `move`（删除源） |
| `confirm` | 必填 `"RESTRUCTURE"` |

## ops 操作类型

| action | 说明 | 必填字段 |
|--------|------|---------|
| `createTitle` | 创建目录节点 | `title` |
| `appendNode` | 挂载文档到目录 | `doc_ids`（JSON 数组字符串） |
| `removeNode` | 删除目录节点 | `node_uuid` |
| `moveNode` | 移动节点（remove+append） | `node_uuid`，可选 `target_uuid` 或 `target_title` |
| `copyDoc` | 跨库复制文档 | `doc_id` |

## 返回

```json
{
  "book_id": "79841574",
  "target_book_id": "80346623",
  "mode": "copy",
  "total": 4,
  "success": 4,
  "failed": 0,
  "results": [...]
}
```

## 别干的事

- 别在 ops 里混 `copyDoc` 和本库操作（copyDoc 只在跨库时有效）
- 别忘记 `confirm` 必须传 `"RESTRUCTURE"`
- `appendNode` 对已存在文档不会改变 parent_uuid，移动文档用 `moveNode`

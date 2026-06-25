> **📌 输出裁剪**：所有工具默认返回精简字段（去除 `_serializer`、`type` 等元数据）。传 `raw=true` 可获取全量原始 JSON。
> 

# 语雀 OpenAPI 参考

> 基地址：`https://www.yuque.com/api/v2`

## 删除文档

```http
DELETE /api/v2/repos/{book_id}/docs/{id}
```

等价于 `DELETE /api/v2/repos/{group_login}/{book_slug}/docs/{id}`。

**用途**：删除指定文档，移入回收站。

### 参数

| 参数 | 位置 | 类型 | 说明 |
|------|------|------|------|
| `book_id` | path | string | 知识库 ID 或 namespace（必填） |
| `id` | path | string | 文档 ID 或 slug（必填） |

### 返回结构

返回被删除文档的 V2DocDetail。

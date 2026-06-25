> **📌 输出裁剪**：所有工具默认返回精简字段（去除 `_serializer`、`type` 等元数据）。传 `raw=true` 可获取全量原始 JSON。
> 

# 语雀 OpenAPI 参考

> 基地址：`https://www.yuque.com/api/v2`

## 更新文档

```http
PUT /api/v2/repos/{book_id}/docs/{id}
Content-Type: application/json

{"title": "新标题", "body": "新内容"}
```

等价于 `PUT /api/v2/repos/{group_login}/{book_slug}/docs/{id}`。

**用途**：更新指定文档的标题、正文、路径等。所有 body 参数可选，只传需要更新的字段。

### 参数

| 参数 | 位置 | 类型 | 说明 |
|------|------|------|------|
| `book_id` | path | string | 知识库 ID 或 namespace（必填） |
| `id` | path | string | 文档 ID 或 slug（必填） |
| `title` | body | string | 标题 |
| `slug` | body | string | 文档路径 |
| `format` | body | string | 内容格式：markdown / html / lake |
| `body` | body | string | 正文内容 |
| `public` | body | int | 0=私密 / 1=公开 / 2=企业内公开 |

### 返回结构

同创建文档返回的 V2DocDetail。

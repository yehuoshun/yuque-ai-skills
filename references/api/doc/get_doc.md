> **📌 输出裁剪**：所有工具默认返回精简字段（去除 `_serializer`、`type` 等元数据）。传 `raw=true` 可获取全量原始 JSON。
> 

# 语雀 OpenAPI 参考

> 基地址：`https://www.yuque.com/api/v2`

## 获取文档详情

```http
GET /api/v2/repos/docs/{id}
```

也可通过 `GET /api/v2/repos/{book_id}/docs/{id}` 或 `GET /api/v2/repos/{group_login}/{book_slug}/docs/{id}` 访问，此时 `id` 支持 slug。

**用途**：获取文档完整内容，包括正文、格式、元信息等。

### 参数

| 参数 | 位置 | 类型 | 说明 | 默认 |
|------|------|------|------|------|
| `id` | path | string | 文档 ID（必填） | - |
| `page_size` | query | int | 数据表分页大小 | 100（1-200） |
| `page` | query | int | 数据表页码 | 1（≥1） |

### 返回结构

```json
{
  "data": {
    "id": 0,
    "type": "Doc",
    "slug": "string",
    "title": "string",
    "description": "string",
    "cover": "string",
    "user_id": 0,
    "book_id": 0,
    "last_editor_id": 0,
    "format": "markdown",
    "body_draft": "string",
    "body": "string",
    "body_sheet": "string",
    "body_table": "string",
    "body_html": "string",
    "body_lake": "string",
    "public": 0,
    "status": "string",
    "likes_count": 0,
    "read_count": 0,
    "hits": 0,
    "comments_count": 0,
    "word_count": 0,
    "created_at": "2019-08-24T14:15:22Z",
    "updated_at": "2019-08-24T14:15:22Z",
    "content_updated_at": "2019-08-24T14:15:22Z",
    "published_at": "2019-08-24T14:15:22Z",
    "first_published_at": "2019-08-24T14:15:22Z",
    "book": {},
    "user": {},
    "creator": {},
    "tags": {},
    "latest_version_id": 0
  }
}
```

### 返回字段

同创建文档返回的 V2DocDetail，另增以下字段：

| 字段 | 类型 | 说明 |
|------|------|------|
| `body_draft` | string | 草稿内容 |
| `body_sheet` | string | 表格正文（type=Sheet 时） |
| `body_table` | string | 数据表正文（type=Table 时） |

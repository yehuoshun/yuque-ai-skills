> **📌 输出裁剪**：所有工具默认返回精简字段（去除 `_serializer`、`type` 等元数据）。传 `raw=true` 可获取全量原始 JSON。
> 

# 语雀 OpenAPI 参考

> 基地址：`https://www.yuque.com/api/v2`

## 创建文档

```http
POST /api/v2/repos/{book_id}/docs
Content-Type: application/json

{"title": "标题", "body": "正文内容", "format": "markdown"}
```

等价于 `POST /api/v2/repos/{group_login}/{book_slug}/docs`。

**用途**：在指定知识库中创建新文档。

> ✅ 创建后自动追加到知识库目录末尾。如果自动追加失败，工具会返回警告信息。

### 参数

| 参数 | 位置 | 类型 | 说明 | 默认 |
|------|------|------|------|------|
| `book_id` | path | string | 知识库 ID 或 namespace（必填） | - |
| `title` | body | string | 标题 | `"无标题"` |
| `slug` | body | string | 文档路径，不填自动生成 | - |
| `format` | body | string | 内容格式：markdown / html / lake | `"markdown"` |
| `body` | body | string | 正文内容（必填） | - |
| `public` | body | int | 0=私密 / 1=公开 / 2=企业内公开，不填继承知识库 | - |

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

### 返回字段 (V2DocDetail)

同 V2Doc，外加以下字段：

| 字段 | 类型 | 说明 |
|------|------|------|
| `format` | string | 内容格式：markdown / lake / html |
| `body` | string | 正文原始内容 |
| `body_draft` | string | 正文草稿内容 |
| `body_html` | string | 正文 HTML 格式 |
| `body_lake` | string | 正文 Lake 格式 |
| `body_sheet` | string | 表格正文（type=Sheet 时返回） |
| `body_table` | string | 数据表正文（type=Table 时返回） |
| `creator` | object | V2User，创建者 |

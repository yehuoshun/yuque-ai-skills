> **📌 输出裁剪**：所有工具默认返回精简字段（去除 `_serializer`、`type` 等元数据）。传 `raw=true` 可获取全量原始 JSON。
> 

# 语雀 OpenAPI 参考

> 基地址：`https://www.yuque.com/api/v2`

## 获取文档历史版本

```http
GET /api/v2/doc_versions?doc_id={doc_id}
```

**用途**：获取文档最近 100 个已发布版本，按时间倒序。

### 参数

| 参数 | 位置 | 类型 | 说明 |
|------|------|------|------|
| `doc_id` | query | int | 文档 ID（必填） |

### 返回结构

```json
{
  "data": [
    {
      "id": 0,
      "doc_id": 0,
      "slug": "string",
      "title": "string",
      "user_id": 0,
      "created_at": "2019-08-24T14:15:22Z",
      "updated_at": "2019-08-24T14:15:22Z",
      "user": {}
    }
  ]
}
```

### 返回字段 (V2DocVersion)

| 字段 | 类型 | 说明 |
|------|------|------|
| `id` | int | 版本 ID |
| `doc_id` | int | 文档 ID |
| `slug` | string | 该版本时的路径 |
| `title` | string | 该版本时的标题 |
| `user_id` | int | 发版人 ID |
| `created_at` | string | 创建时间 (ISO 8601) |
| `updated_at` | string | 更新时间 (ISO 8601) |
| `user` | object | V2User，发版人 |

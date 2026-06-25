> **📌 输出裁剪**：所有工具默认返回精简字段（去除 `_serializer`、`type` 等元数据）。传 `raw=true` 可获取全量原始 JSON。
> 

# 语雀 OpenAPI 参考

> 基地址：`https://www.yuque.com/api/v2`

## 获取文档历史版本详情

```http
GET /api/v2/doc_versions/{id}
```

**用途**：获取指定版本的完整内容，包括正文和 diff。

### 参数

| 参数 | 位置 | 类型 | 说明 |
|------|------|------|------|
| `id` | path | int | 版本 ID（必填） |

### 返回结构

```json
{
  "data": {
    "id": 0,
    "doc_id": 0,
    "slug": "string",
    "title": "string",
    "user_id": 0,
    "format": "markdown",
    "body": "string",
    "body_html": "string",
    "body_asl": "string",
    "diff": "string",
    "created_at": "2019-08-24T14:15:22Z",
    "updated_at": "2019-08-24T14:15:22Z",
    "user": {}
  }
}
```

### 返回字段 (V2DocVersionDetail)

同 V2DocVersion，外加以下字段：

| 字段 | 类型 | 说明 |
|------|------|------|
| `format` | string | 内容格式 |
| `body` | string | Markdown 正文 |
| `body_html` | string | HTML 正文 |
| `body_asl` | string | Lake 格式正文 |
| `diff` | string | 版本差异 |
### yuque_embed_url

```
无（纯工具函数，基于语雀嵌入文档阅读器规范）
```

根据文档 URL 或 doc_id + book 信息，生成带 `view=doc_embed` 参数的嵌入阅读器 URL 和 iframe 代码。

**参数**：`url` 或 `doc_id+book_id`（二选一）、`from`（必填，appname）、`title`（0/1）、`outline`（0/1）、`translate`（语种代码）

**响应**：`embed_url`（嵌入 URL）、`iframe_code`（iframe 标签）、`params`（参数摘要）、`supported_languages`（支持的翻译语种）

**限制**：仅支持公开文档，私密文档需通过 API 获取内容后自行渲染。

参考：[语雀嵌入文档阅读页 API 文档](https://www.yuque.com/yuque/developer/embed)

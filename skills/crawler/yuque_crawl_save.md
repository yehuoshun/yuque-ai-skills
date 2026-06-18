# yuque_crawl_save

去重 + 写入语雀。接收 Agent 清洗后的内容，不负责 fetch/extract。

## 端点

- `POST /repos/{book_id}/docs` — 创建文档
- `PUT /repos/{book_id}/toc` — 加入目录

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|:--:|------|
| `url` | string | ✅ | 来源 URL（用于去重 slug + 来源脚注） |
| `title` | string | ✅ | 文档标题 |
| `body` | string | ✅ | HTML/Markdown 正文内容 |
| `source` | string | ❌ | 来源标识，路由到 `crawler.namespaces.{source}` |
| `target_repo` | string | ❌ | 目标知识库 ID，覆盖配置 |
| `kv_namespace` | string | ❌ | KV 命名空间，默认取 source 或 `crawler` |
| `format` | string | ❌ | 内容格式：`html`（默认）、`markdown`、`lake` |
| `raw` | boolean | ❌ | 返回原始完整 JSON |

## 配置

```json
{
  "crawler": {
    "enabled": true,
    "namespaces": {
      "cnblogs": {
        "book_id": 80197497,
        "kv_slugs": ["80197550/274164064"],
        "schedule_slugs": ["80278170/274358465"]
      }
    }
  }
}
```

目标知识库优先级：`target_repo` 参数 → `crawler.namespaces.{source}.book_id`

## 去重

- `kv.enabled = true` 时启用
- slug = URL 的 md5 前 12 位
- 通过 KV map 检查，已存在则返回 `skipped`

## 使用流程

```
yuque_crawl_fetch → Agent 清洗 → yuque_crawl_save
```

Agent 必须先 fetch HTML，提取正文，转换为 Markdown，再传给 save。详见 `references/api/crawler_api.md`。

## 返回

```json
{
  "status": "saved",
  "url": "https://example.com/post/123",
  "title": "[博客园] 文章标题",
  "slug": "a1b2c3d4e5f6",
  "doc_id": 123456,
  "target_repo": 79841574,
  "kv_namespace": "cnblogs",
  "body_size": 5678
}
```

重复跳过：
```json
{
  "status": "skipped",
  "reason": "duplicate",
  "url": "...",
  "title": "...",
  "slug": "..."
}
```

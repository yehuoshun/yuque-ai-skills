# yuque_crawl_save

抓取 → 提取 → 去重 → 写入语雀，一站式管道。

## 端点

- `POST /repos/{book_id}/docs` — 创建文档
- `PUT /repos/{book_id}/toc` — 加入目录
- `GET /repos/{kv_repo}/docs/{slug}` — KV 去重检查
- `POST /repos/{kv_repo}/docs` — 创建 KV 标记

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|:--:|------|
| `url` | string | ✅ | 目标 URL |
| `source` | string | ❌ | 来源标识，用于路由到 `crawler.sources.{source}` 知识库 |
| `target_repo` | string | ❌ | 目标知识库 ID/namespace，覆盖配置 |
| `kv_repo` | string | ❌ | KV 去重知识库 ID/namespace，覆盖配置 |
| `content_selector` | string | ❌ | 内容区域 CSS 选择器，不传则提取整页 |
| `title_selector` | string | ❌ | 标题 CSS 选择器，不传则用 `<title>` 标签 |
| `headers` | string | ❌ | 自定义请求头，JSON 字符串 |
| `timeout` | number | ❌ | 超时 ms（默认 15000，最大 30000） |
| `title_prefix` | string | ❌ | 标题前缀，如 `[博客园] ` |
| `mode` | string | ❌ | `save`（默认，写入语雀）或 `preview`（仅预览不写入） |
| `raw` | boolean | ❌ | 返回原始完整 JSON |

## 配置

```json
{
  "crawler": {
    "enabled": true,
    "default_repo": { "id": 80197497 },
    "sources": {
      "cnblogs": { "id": 80197497 }
    }
  }
}
```

目标知识库优先级：`target_repo` 参数 → `crawler.sources.{source}` → `crawler.default_repo`

## 去重

- `kv.enabled = true` 时启用 KV slug 去重
- slug = URL 的 md5 前 12 位
- 已存在的文档跳过不写入

## 返回

**save 模式**：
```json
{
  "status": "saved",
  "url": "https://example.com/post/123",
  "title": "[博客园] 文章标题",
  "slug": "a1b2c3d4e5f6",
  "doc_id": 123456,
  "target_repo": "80197497",
  "kv_repo": "80197550",
  "body_size": 5678,
  "elapsed_ms": 345
}
```

**preview 模式**：
```json
{
  "mode": "preview",
  "url": "https://example.com/post/123",
  "title": "文章标题",
  "slug": "a1b2c3d4e5f6",
  "body_size": 5678,
  "body_preview": "前500字符...",
  "elapsed_ms": 345,
  "is_duplicate": false
}
```

**重复跳过**：
```json
{
  "status": "skipped",
  "reason": "duplicate",
  "url": "https://example.com/post/123",
  "title": "文章标题",
  "slug": "a1b2c3d4e5f6"
}
```

## 注意事项

- 自动去除 `<script>`、`<style>`、`<noscript>` 标签
- 简易 HTML→Markdown 转换（`<br>`→换行、`<p>`→段落等）
- 不处理 JS 渲染页面
- 创建文档后自动加入知识库目录

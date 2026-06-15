# yuque_import_url — 从网页 URL 导入文档

## 功能

从任意网页 URL 抓取内容，清洗后导入语雀知识库。一把梭：抓取 → 清洗 → 创建文档 → 挂载 TOC。

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `book_id` | string | ✅ | 目标知识库 ID 或 namespace |
| `url` | string | ✅ | 网页 URL |
| `paths` | string | ✅ | JSON 数组，目录路径列表，如 `["收集/技术文章"]`，1-5 条 |
| `title` | string | ❌ | 文档标题，默认取页面 `<title>` |
| `format` | string | ❌ | 内容格式：markdown / html，默认 markdown |
| `raw` | boolean | ❌ | 返回原始 JSON |

## 网页清洗逻辑

- 移除 script / style / nav / footer / header / aside / iframe
- 优先提取 `<article>` 或 `<main>` 内容
- HTML 实体解码（`&amp;` → `&`）
- 合并连续空行
- 内容上限 100KB（超出截断）
- 自动在文末追加源链接

## 返回

```json
{
  "source_url": "https://example.com/article",
  "source_title": "页面标题",
  "title": "页面标题",
  "book_id": "xxx",
  "paths": ["收集/技术文章"],
  "results": [
    { "path": "收集/技术文章", "doc_id": 123, "slug": "xxx" }
  ],
  "total": 1,
  "success": 1,
  "failed": 0
}
```

## 注意事项

- 一把梭模式，不需要 Agent 来回倒
- 网页内容不可控，清洗逻辑尽量保守
- 目录缓存 TTL 30 分钟
# yuque_import_url — 从网页 URL 导入文档

## 功能

从任意网页 URL 抓取内容，Agent 清洗后导入语雀知识库。工具负责抓取网页 + 建 TOC 目录 + 创建文档 + 挂载，清洗由 Agent 完成。

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `book_id` | string | ✅ | 目标知识库 ID 或 namespace |
| `url` | string | 模式2 | 网页 URL。工具抓取后返回原始内容，Agent 清洗后再调手动模式 |
| `title` | string | 模式1 | 文档标题（Agent 清洗后） |
| `body` | string | 模式1 | 文档内容（Agent 清洗后） |
| `format` | string | ❌ | 内容格式：markdown / html，默认 markdown |
| `paths` | string | ✅ | JSON 数组，目录路径列表，如 `["收集/技术文章"]`，1-5 条 |
| `source_url` | string | ❌ | 源链接，追尾为页脚。模式2 自动填入 |
| `source_title` | string | ❌ | 源标题，用于页脚链接 |
| `raw` | boolean | ❌ | 返回原始 JSON（默认 false） |

## 双模式流程

### 模式2：fetch-only（首次调用）

```
Agent 调用 → 传入 url + book_id + paths
         → 工具抓取网页，提取标题 + 正文
         → 返回 { mode: "fetch-only", title, body, format, source_url, source_title, hint }
         → Agent 阅读内容，智能清洗（去广告/导航/评论区/脚本）
         → Agent 重组为干净的 Markdown
```

### 模式1：手动创建（清洗后调用）

```
Agent 调用 → 传入 title + body + format + paths + source_url
         → 工具建目录 + 创建文档 + 挂载 TOC + 追尾源链接
```

## 网页提取逻辑

- 移除 script / style / nav / footer / header / aside 标签
- 优先提取 `<article>` 或 `<main>` 内容
- 解码 HTML 实体（`&amp;` → `&` 等）
- 内容上限 100KB（超出截断）
- 提取失败时降级返回 og:description / meta description

## 返回

### fetch-only 模式

```json
{
  "mode": "fetch-only",
  "url": "https://example.com/article",
  "title": "页面标题",
  "body": "提取的正文内容...",
  "format": "markdown",
  "source_url": "https://example.com/article",
  "source_title": "页面标题",
  "hint": "Agent 清洗内容后，调用 yuque_import_url 手动模式创建文档"
}
```

### 手动模式

```json
{
  "mode": "manual",
  "source_url": "https://example.com/article",
  "title": "文档标题",
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

- 网页内容不可控（广告、导航、评论区），清洗必须由 Agent 智能判断
- 工具只做结构化提取（去标签），不做语义清洗
- 传入 `source_url` 时自动在 body 末尾追加源链接
- 目录缓存 TTL 30 分钟，同库同路径不重复创建
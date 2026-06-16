# yuque_crawl_blog

> 端点：无（HTTP 抓取 + cheerio 转换）
> 说明：博客园文章一站式抓取→清洗→写入。内部用 cheerio 做 HTML→Markdown 转换，代码块自动识别语言并生成语法高亮。

## 参数

| 参数 | 类型 | 必填 | 默认 | 说明 |
|------|------|:--:|------|------|
| url | string | ✅ | - | 博客园文章 URL |
| source | string | ❌ | `cnblogs` | 数据源 key |
| target_repo | string | ❌ | config | 目标知识库 ID |
| content_selector | string | ❌ | `#cnblogs_post_body` | 正文 CSS 选择器 |
| title_prefix | string | ❌ | `[博客园] ` | 标题前缀 |
| timeout | number | ❌ | 15000 | 超时 ms（上限 30000） |
| mode | string | ❌ | `save` | `save` / `preview` |

## 响应

### preview 模式

```json
{
  "mode": "preview",
  "url": "https://www.cnblogs.com/xxx/p/123",
  "title": "[博客园] 文章标题",
  "body_size": 8719,
  "body_preview": "> 原文链接：...",
  "elapsed_ms": 134
}
```

### save 模式

```json
{
  "status": "saved",
  "url": "https://www.cnblogs.com/xxx/p/123",
  "title": "[博客园] 文章标题",
  "slug": "a56512aa6913",
  "doc_id": 274141819,
  "target_repo": "80197497",
  "body_size": 8719,
  "elapsed_ms": 102
}
```

## 转换规则

工具内部用 cheerio 解析 HTML，输出标准 Markdown：

- 标题：`<h1>` → `#`，`<h2>` → `##`，以此类推
- 代码块：`<pre><code class="language-xxx">` → ` ```xxx `
- 内联代码：`<code>` → `` ` ``
- 表格：`<table>` → Markdown 表格
- 列表：`<ul>` → `- `，`<ol>` → `1. `
- 粗体/斜体：`<strong>` → `**`，`<em>` → `*`
- 链接/图片：`<a>/<img>` → `[]()`/`![]()`
- 清理：去除 `<script>/<style>/<noscript>`，`data-src` → `src`

## 去重

- 相同 URL 的 md5 前 12 位作为 slug
- 写入前先删除旧文档再创建，确保 Markdown 重新解析
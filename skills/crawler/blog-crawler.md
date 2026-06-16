# 博客园爬虫 — Agent 清洗格式化规则

> 工具：`yuque_crawl_fetch` + `yuque_crawl_extract` + `yuque_crawl_save`
> 目标：从博客园首页抓取文章列表 → 逐篇提取正文 → 清洗格式化 → 写入语雀

## 流程

```
1. crawl_fetch(首页) → HTML
2. crawl_extract(HTML, "article.post-item a.post-item-title", attr="href") → 文章链接列表
3. 逐篇：
   a. crawl_save(url, source="cnblogs", content_selector="#cnblogs_post_body", title_prefix="[博客园] ") → 写入语雀
   b. 拿到 doc_id 后，调 yuque_update_doc 清洗格式化
```

## 清洗格式化规则

拿到 `crawl_save` 写入的 HTML 文档后，用 `yuque_update_doc` 重新格式化。规则如下：

### 1. 来源链接
文档首行必须是可点击的来源超链接：
```markdown
> 原文链接：[文章标题](原始URL)
```
⚠️ 不要用纯文本，必须是 Markdown 链接格式。

### 2. 图片处理
- 博客园图片使用 `data-src` 懒加载，需替换为真实 `src`
- 所有 `<img data-src="...">` → `![](真实URL)`
- 图片 alt 文本保留

### 3. 代码块
- `<pre><code>` → Markdown 围栏代码块 ` ``` `
- 内联 `<code>` → `` ` ``
- 保留原始代码内容，不做转义

### 4. 标题层级
- `<h2>` → `##`，`<h3>` → `###`，以此类推
- 标题前后保留空行

### 5. 表格
- `<table>` → Markdown 表格格式
- 表头加分隔线 `|---|---|`

### 6. 列表
- `<ul><li>` → `- `
- `<ol><li>` → `1. ` `2. ` 等
- 嵌套列表保留缩进

### 7. 粗体/斜体
- `<strong>` → `**文本**`
- `<em>` → `*文本*`

### 8. 引用
- `<blockquote>` → `> 引用内容`

### 9. 清理
- 删除 `<script>`、`<style>`、`<noscript>` 标签
- 删除零宽字符 `\u200b`
- 删除多余空行（连续 3 个以上空行合并为 2 个）
- 删除导航、侧边栏、页脚等非正文内容

### 10. 文档格式
- 语雀文档 `format` 设为 `markdown`
- 标题保留 `[博客园] ` 前缀
- 文档末尾保留原文链接

## 示例

清洗前（crawl_save 原始输出）：
```html
> 来源：https://www.cnblogs.com/xxx | 抓取时间：...

<div id="cnblogs_post_body">
<p><strong>重要概念</strong>：这是一段文字。</p>
<h2>标题</h2>
<pre><code>console.log("hello");</code></pre>
<img data-src="https://img2024.cnblogs.com/xxx.png" alt="示例图">
</div>
```

清洗后（Agent 格式化）：
```markdown
> 原文链接：[文章标题](https://www.cnblogs.com/xxx)

**重要概念**：这是一段文字。

## 标题

\`\`\`
console.log("hello");
\`\`\`

![示例图](https://img2024.cnblogs.com/xxx.png)
```

## 注意事项

- 如果 `crawl_save` 返回 `status: "skipped"`（重复），跳过该篇
- 批量抓取时控制频率，每篇间隔 1-2 秒
- 清洗后调 `yuque_update_doc` 更新文档，不要创建新文档
- 如果清洗后内容为空或过短（<100 字符），保留原始 HTML 不覆盖
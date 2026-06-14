# yuque_copy_doc — 单文档跨库复制

## 功能

将文档复制到目标知识库。支持两种模式：

1. **fetch-only 模式**（`doc_id` + `source_book_id`）：工具拉取源文档，返回原始内容（title/body/format）给 Agent。Agent 清洗内容后调用手动模式创建。解决大文档（>30KB）通过 mcporter CLI 传 body 不稳定的问题。
2. **手动模式**（`title/body/format/paths`）：Agent 传入清洗后的内容，工具建目录+创建文档+挂载 TOC。

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `target_book_id` | string | ✅ | 目标知识库 ID 或 namespace |
| `paths` | string | ✅ | JSON 数组，目录路径列表，如 `["Java/Spring","Database/MySQL"]`，1-5 条 |
| `doc_id` | number | fetch-only 模式✅ | 源文档 ID。传入后工具拉取源文档并返回原始内容 |
| `source_book_id` | string | fetch-only 模式✅ | 源知识库 ID 或 namespace |
| `title` | string | 手动模式✅ | 文档标题 |
| `body` | string | 手动模式✅ | 文档内容（Agent 清洗后） |
| `format` | string | 手动模式✅ | 内容格式：markdown / lake / html |
| `source_url` | string | ❌ | 源文档 URL，追尾为页脚链接 |
| `source_title` | string | ❌ | 源文档标题，用于页脚链接 |
| `raw` | boolean | ❌ | 返回原始 JSON（默认 false） |

## 流程

### fetch-only 模式（推荐，处理大文档）
1. Agent 调用 `yuque_copy_doc`，传入 `doc_id` + `source_book_id` + `target_book_id` + `paths`
2. 工具拉取源文档，返回 `{mode:"fetch-only", title, body, format, source_url, source_title}`
3. Agent 阅读内容，智能判断保留哪些内容、丢弃哪些垃圾（如论坛剪藏的 UI 元素）
4. Agent 重组为干净的 Markdown
5. Agent 调用手动模式（传入清洗后的 title/body/format/paths + source_url），工具创建文档

### 手动模式
1. Agent 确定内容 → 清洗 → 组织为 Markdown
2. 调用本工具，传入 title/body/format/paths
3. 工具逐路径建目录链（TOC TITLE 节点，30 分钟缓存）
4. 每个路径下创建文档副本并挂载

## 返回

### fetch-only 模式
```json
{
  "mode": "fetch-only",
  "source_doc_id": 123456,
  "source_book_id": "28105968",
  "title": "文档标题",
  "body": "原始 Markdown 内容...",
  "format": "markdown",
  "source_url": "https://www.yuque.com/...",
  "source_title": "文档标题",
  "hint": "Agent 清洗内容后，调用 yuque_copy_doc 手动模式（title/body/format/paths）创建文档"
}
```

### 手动模式
```json
{
  "mode": "manual",
  "source_doc_id": null,
  "source_book_id": null,
  "title": "文档标题",
  "target_book_id": "xxx",
  "paths": ["Java/Spring", "Database/MySQL"],
  "results": [
    { "path": "Java/Spring", "doc_id": 123, "slug": "xxx" },
    { "path": "Database/MySQL", "doc_id": 124, "slug": "xxx" }
  ],
  "total": 2,
  "success": 2,
  "failed": 0
}
```

## 注意事项

- **大文档（>30KB）请用 fetch-only 模式**，避免 mcporter CLI 参数长度限制
- **清洗由 Agent 负责**，工具只做搬运。Agent 根据内容智能判断保留哪些、丢弃哪些
- 手动模式传入 `source_url` 时自动在 body 末尾追加源链接
- 图片/附件 URL 原样保留（CDN 全局可见）
- 目录缓存 TTL 30 分钟，同库同路径不重复创建
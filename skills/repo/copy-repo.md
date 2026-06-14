# yuque_copy_repo — 批量跨库复制

## 功能

将 Agent 清洗后的多篇文档批量复制到目标知识库。工具负责串行建 TOC 目录 + 创建文档 + 挂载，清洗和分类由 Agent 完成。

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `target_book_id` | string | ✅ | 目标知识库 ID 或 namespace |
| `documents` | string | ✅ | JSON 数组，每项为 `{title, body, format, paths, source_url?, source_title?}`，paths 为 1-5 条目录路径 |
| `raw` | boolean | ❌ | 返回原始 JSON（默认 false） |

## 流程

1. Agent 拉取源库文档 → 逐篇清洗内容 → 确定每篇目录路径 → 组装 documents 数组
2. 调用本工具，传入 target_book_id + documents
3. 工具串行逐篇处理：建目录链 → 创建文档 → 挂载到 TOC
4. 全局目录缓存，同库同路径不重复创建

## 返回

```json
{
  "target_book_id": "xxx",
  "total_docs": 50,
  "total_copies": 120,
  "total_failed": 3,
  "details": [
    {
      "title": "文档标题",
      "paths": ["Java/Spring", "Database/MySQL"],
      "results": [
        { "path": "Java/Spring", "doc_id": 123, "slug": "xxx" },
        { "path": "Database/MySQL", "doc_id": 124, "slug": "xxx" }
      ]
    }
  ]
}
```

## 注意事项

- 串行处理，避免 OOM，大库复制可能耗时较长
- 清洗和分类由 Agent 负责，工具只做搬运
- 图片/附件 URL 原样保留（CDN 全局可见）
- 传入 `source_url` 时自动在 body 末尾追加源链接
- 目录缓存 TTL 30 分钟，同库同路径不重复创建
- documents 每项 paths 最多 5 条

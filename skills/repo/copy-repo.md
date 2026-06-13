# yuque_copy_repo — 批量跨库复制

## 功能

将源知识库的文档批量复制到目标知识库。支持按 doc_ids 筛选，不传则复制全部文档。每篇文档都会经过 LLM 分类后挂载到目标库的对应目录下。

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `source_book_id` | string | ✅ | 源知识库 ID 或 namespace |
| `target_book_id` | string | ✅ | 目标知识库 ID 或 namespace |
| `doc_ids` | string | ❌ | 逗号分隔的文档 ID 列表，不传则复制全部 |
| `raw` | boolean | ❌ | 返回原始 JSON（默认 false） |

## 流程

1. 获取源库文档列表（全量或按 doc_ids 筛选）
2. 串行逐篇处理：
   - 拉取文档 content
   - 清洗（去剪藏垃圾）
   - LLM 分类
   - 目标库建目录 → 创建副本
3. 全局目录缓存，避免重复创建

## 返回

```json
{
  "source_book_id": "xxx",
  "target_book_id": "xxx",
  "total_docs": 50,
  "total_copies": 120,
  "total_failed": 3,
  "details": [
    {
      "source_doc_id": "xxx",
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
- 文档多目录归属时，每个目录下都会创建一份副本
- 图片/附件 URL 原样保留（CDN 全局可见）
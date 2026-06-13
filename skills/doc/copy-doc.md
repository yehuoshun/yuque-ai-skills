# yuque_copy_doc — 单文档跨库复制

## 功能

将源知识库的一篇文档复制到目标知识库。自动清洗剪藏网页的垃圾标签，通过 LLM 分析文档内容自动分类到对应目录路径下，并在每个分类路径下创建文档副本。

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `doc_id` | string | ✅ | 源文档 ID |
| `target_book_id` | string | ✅ | 目标知识库 ID 或 namespace |
| `raw` | boolean | ❌ | 返回原始 JSON（默认 false） |

## 流程

1. 拉取源文档（title + content + tags）
2. 清洗 content（去剪藏垃圾标签、style/script、隐藏元素）
3. LLM 分析文档内容 → 输出目录路径列表（1-5 条）
4. 目标库逐路径建目录链（缓存已建目录）
5. 每个路径下创建文档副本

## 目录分类规则

- 从标题、关键词、代码块语言、技术栈推断归属
- 路径层级不超过 3 级，格式 `一级/二级/三级`
- 无 LLM 配置时 fallback 到关键词匹配

## 返回

```json
{
  "source_doc_id": "xxx",
  "source_title": "文档标题",
  "target_book_id": "xxx",
  "paths": ["Java/Spring/SpringBoot", "Database/MySQL"],
  "results": [
    { "path": "Java/Spring/SpringBoot", "doc_id": 123, "slug": "xxx" },
    { "path": "Database/MySQL", "doc_id": 124, "slug": "xxx" }
  ],
  "total": 2,
  "success": 2,
  "failed": 0
}
```

## 注意事项

- 图片/附件 URL 原样保留（CDN 全局可见）
- 文档格式（lake/markdown/html）保持不变
- 剪藏网页的 style/script/hidden 元素会被清除

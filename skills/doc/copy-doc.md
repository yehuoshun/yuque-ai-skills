# yuque_copy_doc — 单文档跨库复制

## 功能

将 Agent 清洗后的文档内容复制到目标知识库。工具负责建 TOC 目录 + 创建文档 + 挂载，清洗和分类由 Agent 完成。

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `target_book_id` | string | ✅ | 目标知识库 ID 或 namespace |
| `title` | string | ✅ | 文档标题 |
| `body` | string | ✅ | 文档内容（Agent 清洗后） |
| `format` | string | ✅ | 内容格式：markdown / lake / html |
| `paths` | string | ✅ | JSON 数组，目录路径列表，如 `["Java/Spring","Database/MySQL"]`，1-5 条 |
| `source_url` | string | ❌ | 源文档 URL，追尾为页脚链接 |
| `source_title` | string | ❌ | 源文档标题，用于页脚链接 |
| `raw` | boolean | ❌ | 返回原始 JSON（默认 false） |

## 流程

1. Agent 拉取源文档 → 清洗内容 → 确定目录路径
2. 调用本工具，传入清洗后的 title/body/format/paths
3. 工具逐路径建目录链（TOC TITLE 节点，30 分钟缓存）
4. 每个路径下创建文档副本
5. 文档挂载到对应目录节点下

## 返回

```json
{
  "title": "文档标题",
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

- 清洗和分类由 Agent 负责，工具只做搬运
- 图片/附件 URL 原样保留（CDN 全局可见）
- 传入 `source_url` 时自动在 body 末尾追加源链接
- 目录缓存 TTL 30 分钟，同库同路径不重复创建

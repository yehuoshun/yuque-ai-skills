# yuque_copy_doc — 单文档跨库复制

## 功能

将文档复制到目标知识库。支持两种模式：

1. **手动模式**：Agent 清洗内容后传入 title/body/format/paths，工具建目录+创建
2. **自动拉取模式**（推荐）：Agent 传入 doc_id，工具内部拉取源文档（解决大文档 >30KB 通过 mcporter CLI 传 body 不稳定的问题）

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `target_book_id` | string | ✅ | 目标知识库 ID 或 namespace |
| `paths` | string | ✅ | JSON 数组，目录路径列表，如 `["Java/Spring","Database/MySQL"]`，1-5 条 |
| `doc_id` | number | ❌ | 源文档 ID。传入后自动拉取源文档，忽略 title/body/format（推荐） |
| `source_book_id` | string | ❌ | 源知识库 ID。doc_id 模式下可选，不传则自动从文档所属库获取 |
| `title` | string | 手动模式✅ | 文档标题 |
| `body` | string | 手动模式✅ | 文档内容（Agent 清洗后） |
| `format` | string | 手动模式✅ | 内容格式：markdown / lake / html |
| `source_url` | string | ❌ | 源文档 URL，追尾为页脚链接。doc_id 模式自动生成 |
| `source_title` | string | ❌ | 源文档标题，用于页脚链接。doc_id 模式自动使用源文档标题 |
| `raw` | boolean | ❌ | 返回原始 JSON（默认 false） |

## 流程

### 手动模式
1. Agent 拉取源文档 → 清洗内容 → 确定目录路径
2. 调用本工具，传入清洗后的 title/body/format/paths
3. 工具逐路径建目录链（TOC TITLE 节点，30 分钟缓存）
4. 每个路径下创建文档副本
5. 文档挂载到对应目录节点下

### 自动拉取模式（推荐）
1. Agent 确定 doc_id + target_book_id + paths
2. 工具内部：拉取源文档 → 自动清洗 → 建目录 → 创建 → 挂载
3. 自动追尾源链接、自动填充标题
4. 防止同库复制（SAME_BOOK 错误）

## 返回

```json
{
  "mode": "auto-fetch",
  "source_doc_id": 123456,
  "source_book_id": "yehuoshun/source-repo",
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

- **大文档（>30KB）请用自动拉取模式**，避免 mcporter CLI 参数长度限制
- 清洗和分类由 Agent 负责（手动模式），工具只做搬运
- 图片/附件 URL 原样保留（CDN 全局可见）
- 传入 `source_url` 时自动在 body 末尾追加源链接
- 目录缓存 TTL 30 分钟，同库同路径不重复创建
- 自动拉取模式会阻止同库复制（返回 SAME_BOOK 错误）

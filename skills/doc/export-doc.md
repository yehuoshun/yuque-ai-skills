# yuque_export_doc

> 端点：`GET /api/v2/repos/docs/:id`（获取文档详情后本地导出）
> 说明：导出单篇文档为 Markdown 文件。资源下载已拆分到 `yuque_export_resources`，本工具只管取文档 → 转 Markdown → 加 frontmatter → 写文件。

## 触发场景

- 用户说「导出这篇文档」「下载这篇文档为 Markdown」
- 需要把单篇语雀文档保存到本地

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `id` | string | ✅ | 文档 ID 或 slug |
| `book_id` | string | ❌ | 知识库 ID（使用 slug 时建议提供） |
| `output_dir` | string | ✅ | 输出目录路径。文件保存为 `<output_dir>/<标题>.md` |
| `raw_body` | boolean | ❌ | 是否直接输出原始 body 字段，默认 false。markdown 格式文档适用 |

## 响应

```json
{
  "status": "done",
  "doc": { "id": "123", "slug": "my-doc", "title": "文档标题", "format": "markdown" },
  "output_dir": "/tmp/yuque-export/my-doc",
  "file": "/tmp/yuque-export/my-doc/文档标题.md",
  "word_count": 1500,
  "note": "图片/附件引用保留原始 URL。如需下载资源，请调用 yuque_export_resources"
}
```

## 与 yuque_export_repo 的区别

| | yuque_export_doc | yuque_export_repo |
|---|---|---|
| 导出范围 | 单篇文档 | 整个知识库 |
| 资源下载 | 独立工具（yuque_export_resources） | 内嵌 |

## 注意事项

- `output_dir` 为必填参数，用户自行选择导出路径
- 输出文件包含 YAML frontmatter（标题、创建时间、字数等）
- markdown 格式文档直接使用 body 字段，html/lake 格式自动转换为 Markdown
- 图片和附件引用**保留原始 CDN URL**，如需下载到本地请调 `yuque_export_resources`
- 输出文件名以文档标题命名，非法字符替换为 `_`

## 调完干嘛

- 检查 `output_dir` 下的 .md 文件内容是否完整
- 如需下载资源，调 `yuque_export_resources` 获取本地路径映射

## 别干的事

- 不要并发导出同一篇文档
- 不要将 output_dir 设在语雀仓库内
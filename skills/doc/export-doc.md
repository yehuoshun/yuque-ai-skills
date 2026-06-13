# yuque_export_doc

> 端点：`GET /api/v2/repos/docs/:id`（获取文档详情后本地导出）
> 说明：导出单篇文档为 Markdown 文件，支持图片/附件下载与降级

## 触发场景

- 用户说「导出这篇文档」「下载这篇文档为 Markdown」
- 需要把单篇语雀文档保存到本地
- 提取文档中的图片到本地

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `id` | string | ✅ | 文档 ID 或 slug |
| `book_id` | string | ❌ | 知识库 ID（使用 slug 时建议提供） |
| `output_dir` | string | ❌ | 输出目录路径，默认 `./yuque-export/<doc_slug>/` |
| `download_images` | boolean | ❌ | 是否下载图片到本地，默认 true |
| `raw_body` | boolean | ❌ | 是否直接输出原始 body，默认 false |

## 响应

```json
{
  "status": "done",
  "doc": { "id": "123", "slug": "my-doc", "title": "文档标题", "format": "markdown" },
  "output_dir": "/tmp/yuque-export/my-doc",
  "file": "/tmp/yuque-export/my-doc/my-doc.md",
  "images_downloaded": 3,
  "images_failed": 1
}
```

## 与 yuque_export_repo 的区别

| | yuque_export_doc | yuque_export_repo |
|---|---|---|
| 导出范围 | 单篇文档 | 整个知识库 |
| 速度 | 快 | 取决于文档数量 |
| 适用场景 | 快速保存一篇 | 备份/迁移整个知识库 |

## 注意事项

- 图片下载 15 秒超时，超时自动降级为原 CDN 链接
- 输出文件包含 YAML frontmatter（标题、时间、字数等）
- markdown 格式文档直接使用 body 字段，html/lake 格式自动转换

## 调完干嘛

- 检查 `output_dir` 下的 .md 文件内容是否完整
- 检查 `images_failed` 数量，手动补下载失败的图片

## 别干的事

- 不要并发导出同一篇文档
- 不要将 output_dir 设在语雀仓库内

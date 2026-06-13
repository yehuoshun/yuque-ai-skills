# yuque_export_repo

> 端点：批量操作（无对应单一 API 端点）
> 说明：批量导出知识库全部文档为 Markdown 文件，支持图片/附件下载与降级

## 触发场景

- 用户说「导出语雀知识库」「备份知识库」「下载全部文档」
- 需要将语雀文档迁移到本地或其他平台
- 批量获取文档内容做离线分析

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `book_id` | string | ✅ | 知识库 ID 或 namespace（如 `group/book_slug`） |
| `output_dir` | string | ❌ | 输出目录路径，默认 `./yuque-export/<book_slug>/` |
| `download_images` | boolean | ❌ | 是否下载图片到本地，默认 true |
| `raw_body` | boolean | ❌ | 是否直接输出原始 body（不转 Markdown），默认 false |

## 响应

```json
{
  "status": "done",
  "repo": { "id": "xxx", "slug": "xxx", "name": "xxx" },
  "output_dir": "/tmp/yuque-export/xxx",
  "summary": {
    "total_docs": 10,
    "ok": 9,
    "error": 1,
    "images_downloaded": 15,
    "images_failed": 2
  },
  "files": [
    {
      "doc_id": 123,
      "slug": "doc-slug",
      "title": "文档标题",
      "status": "ok",
      "file": "/tmp/yuque-export/xxx/doc-slug.md",
      "images_downloaded": 3,
      "images_failed": 1
    }
  ]
}
```

## 工作原理

1. 分页获取知识库全部文档列表（每页 100 篇）
2. 逐个获取文档详情（包含 body + body_html）
3. 从 body_html 中提取图片/附件 URL
4. 尝试下载图片到 `output_dir/images/`，下载附件到 `output_dir/attachments/`
5. **下载失败降级**：保留原始 CDN 链接，不丢数据
6. 生成 Markdown 文件（含 YAML frontmatter）
7. 返回完整导出报告

## 文档格式处理

| 源格式 | 导出行为 |
|--------|---------|
| `markdown` | 直接使用 body 字段 |
| `html` | body_html → 简单 Markdown 转换 |
| `lake` | body_html → 简单 Markdown 转换 |

传 `raw_body=true` 可跳过转换，直接输出原始 body。

## 注意事项

- 大知识库（>100 篇）导出时间较长，建议设置较长的超时时间
- 图片下载 15 秒超时，超时自动降级为原链接
- 重复 URL 的图片只下载一次
- 输出文件使用文档 slug 命名，非法字符替换为 `_`
- 每个导出的 Markdown 文件头部包含 YAML frontmatter（标题、创建时间、字数等）

## 使用示例

```
导出知识库 yehuoshun/my-docs 到 /tmp/backup/
导出时跳过图片下载以加快速度
```

## 调完干嘛

- 检查 `output_dir` 下的文件是否完整
- 检查报告中 `images_failed` 数量，手动补下载失败的图片
- Markdown 文件可直接用 Obsidian/Typora/VSCode 打开

## 别干的事

- 不要并发导出同一个知识库，会导致重复文件
- 不要将 output_dir 设在语雀仓库内，避免循环

# yuque_import_file — 从本地文件导入文档

## 功能

从本地 Markdown/HTML 文件导入文档到语雀。支持三种模式处理附件和图片。

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_path` | string | ✅ | 本地文件路径（.md 或 .html） |
| `book_id` | string | ✅ | 目标知识库 ID 或 namespace |
| `paths` | string | ✅ | JSON 数组，目录路径列表，1-5 条 |
| `mode` | string | ❌ | 导入模式：direct / upload_assets / embed_assets，默认 direct |
| `title` | string | ❌ | 文档标题，默认取文件名（不含扩展名） |
| `slug` | string | ❌ | 文档 slug，默认自动生成 |
| `format` | string | ❌ | 内容格式：markdown / html，默认 markdown |
| `raw` | boolean | ❌ | 返回原始 JSON |

## 三种模式

### direct（直接导入）

原样导入，不处理附件和图片。本地路径保持不变。

### upload_assets（附件上传）

提取 Markdown 中的本地图片和附件引用 → 上传到语雀 CDN → 在内存中替换路径 → 导入文档。

- 需要配置 Cookie 和 ctoken
- 图片上传到 `type=image`，附件上传到 `type=file`
- 上传失败保留原路径，不阻断导入

### embed_assets（附件转义）

提取附件引用（`.docx`/`.xlsx`/`.pptx`/`.pdf`）→ 解析内容 → 创建为子文档 → 在内存中替换引用为语雀文档链接 → 导入原文档。

- 图片不转义，保留原路径
- 解析依赖外部工具（pdftotext/pandoc/openpyxl/python-pptx），不可用时降级显示提示
- 所有操作在内存中完成，不修改源文件

## 返回

```json
{
  "mode": "embed_assets",
  "source_file": "/path/to/doc.md",
  "title": "doc",
  "book_id": "xxx",
  "paths": ["导入"],
  "assets": {
    "total": 2,
    "uploaded": 0,
    "embedded": 1,
    "failed": 1,
    "details": [
      { "original": "img.png", "type": "image", "error": "文件不存在" },
      { "original": "data.xlsx", "type": "attachment", "embedDocId": 123, "embedDocSlug": "xxx" }
    ]
  },
  "results": [
    { "path": "导入", "doc_id": 124, "slug": "xxx" }
  ],
  "total": 1,
  "success": 1,
  "failed": 0
}
```

## 注意事项

- 所有模式均在内存中处理，不修改源文件
- upload_assets 需要 Cookie 配置，否则报错
- embed_assets 的附件解析依赖系统工具，解析失败时降级为提示文本
- 目录缓存 TTL 30 分钟
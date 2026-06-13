# yuque_import_doc

> 端点：`POST /api/v2/repos/:book_id/docs`（创建文档）+ `POST /api/upload/attach`（上传图片）
> 说明：从本地 Markdown/HTML 文件导入文档到语雀，支持本地图片自动上传

## 触发场景

- 用户说「导入这个文件到语雀」「把本地文档上传到语雀」
- 从 Obsidian/Typora/本地 Markdown 迁移到语雀
- 批量导入本地文档（配合脚本使用）

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `file_path` | string | ✅ | 本地文件路径（.md 或 .html） |
| `book_id` | string | ✅ | 目标知识库 ID 或 namespace |
| `title` | string | ❌ | 文档标题，默认取文件名（去扩展名） |
| `slug` | string | ❌ | 文档 slug，自动生成 |
| `format` | string | ❌ | 内容格式：markdown / html，默认 markdown |
| `public` | number | ❌ | 可见性：0=私密, 1=公开, 2=团队公开 |

## 响应

```json
{
  "status": "done",
  "doc": { "id": 123, "slug": "xxx", "title": "文档标题" },
  "source_file": "/tmp/test.md",
  "images": {
    "total": 3,
    "uploaded": 2,
    "failed": 1,
    "details": [
      { "original": "./images/a.png", "cdnUrl": "https://cdn.nlark.com/xxx" },
      { "original": "./images/b.png", "error": "File not found" }
    ]
  },
  "note": "未配置 Cookie，图片未上传，保留本地路径"
}
```

## 图片处理

1. 解析 Markdown 中的 `![](path)` 和 `<img src="path">` 引用
2. 跳过网络 URL（`http://` / `https://`），只处理本地路径
3. 尝试上传到语雀 CDN（需要 Cookie 认证）
4. **上传成功** → 替换为 CDN URL
5. **上传失败** → 保留原始本地路径（降级）
6. **无 Cookie** → 全部跳过，保留本地路径

## 注意事项

- 图片上传需要 Cookie 认证（`config.json` 中配置 `cookie` + `ctoken`）
- 本地图片路径相对于 Markdown 文件所在目录解析
- 文件编码必须是 UTF-8
- 单文件大小无硬限制，但语雀 API 有 body 长度限制

## 调完干嘛

- 在语雀中打开文档检查格式是否正确
- 检查 `images.failed` 数量，手动补传失败的图片
- 如需批量导入，用脚本循环调用本工具

## 别干的事

- 不要导入二进制文件（PDF/图片等），用 `yuque_upload_attachment` 上传
- 不要并发导入到同一个知识库，slug 可能冲突

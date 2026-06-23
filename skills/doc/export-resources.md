# yuque_export_resources

> 说明：下载文档中的图片/附件到本地目录，返回 URL→本地路径映射
> 职责独立于 `yuque_export_doc`，用户可按需调用。

## 触发场景

- 导出文档后需要把图片/附件也下到本地
- 只需下载资源，不需要重新导出文档
- Agent 编排：先 `yuque_export_doc` 拿 Markdown → 再 `yuque_export_resources` 下资源

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `id` | string | ✅ | 文档 ID 或 slug |
| `book_id` | string | ❌ | 知识库 ID（使用 slug 时建议提供） |
| `output_dir` | string | ✅ | 资源保存目录。资源保存到 `<output_dir>/images/` 和 `<output_dir>/attachments/` |

## 响应

```json
{
  "status": "done",
  "doc": { "id": "123", "title": "文档标题" },
  "output_dir": "/tmp/yuque-export/my-doc",
  "resources_total": 3,
  "downloaded": 2,
  "failed": 1,
  "mapping": [
    { "url": "https://cdn.nlark.com/.../image.png", "localPath": "/tmp/yuque-export/my-doc/images/image.png", "type": "image", "success": true },
    { "url": "https://cdn.nlark.com/.../failed.png", "localPath": "/tmp/yuque-export/my-doc/images/failed.png", "type": "image", "success": false, "error": "HTTP 404" }
  ]
}
```

## 使用建议

- 通常配合 `yuque_export_doc` 使用，Agent 编排流程：
  1. `yuque_export_doc` → 得到 .md 文件（图片为 CDN URL）
  2. `yuque_export_resources` → 下载资源到本地
  3. 替换 .md 文件中的 CDN URL 为本地路径（Agent 侧处理）
- 如果文档无 `body_html`（如纯 markdown 格式），资源数为 0，跳过下载

## 注意事项

- 资源下载 15 秒超时，超时自动降级
- 已存在的文件跳过下载（幂等）
- 仅下载 `<img src="">` 和 `![]()` 格式的图片引用，不处理 CSS/JS 资源

## 调完干嘛

- 检查 `mapping` 中 `failed` 数量，手动补下载失败的资源
- 可用 sed 或 Agent 处理，将 .md 中的 CDN URL 替换为 `localPath`

## 别干的事

- 不要对同一文档并发调用多次
- 不要把 output_dir 设在语雀仓库内
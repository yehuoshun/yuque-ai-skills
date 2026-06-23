# yuque_copy_doc

> 说明：单文档跨知识库复制。Agent 通过 `yuque_get_doc` 拉取源文档 → 清洗内容 → 调用本工具创建副本。

## 触发场景

- 用户说「把这篇文档复制到那个知识库」
- 跨库迁移/整理文档
- 创建文档副本并放到指定目录

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `target_book_id` | string | ✅ | 目标知识库 ID 或 namespace |
| `title` | string | ✅ | 文档标题 |
| `body` | string | ✅ | 文档内容（Agent 清洗后） |
| `format` | string | ✅ | 内容格式：markdown / lake / html |
| `paths` | string | ✅ | JSON 数组，目录路径，如 `["Java/Spring"]`，1-5 个 |
| `source_url` | string | ❌ | 源文档 URL，作为脚注链接追尾 |
| `source_title` | string | ❌ | 源文档标题，用于脚注 |

## 使用流程

```
1. yuque_get_doc(id, raw=true) → 拿到源文档 body
2. Agent 清洗内容（去噪音、修格式）
3. yuque_copy_doc(target_book_id, title, body, format, paths, source_url)
```

## 响应

```json
{
  "title": "文档标题",
  "target_book_id": "80346623",
  "paths": ["Java/Spring"],
  "results": [
    { "path": "Java/Spring", "doc_id": 123456, "slug": "abc123" }
  ],
  "total": 1,
  "success": 1,
  "failed": 0
}
```

## 注意事项

- 传入 `source_url` 时，工具自动在文档末尾追加「📋 源文档」链接
- 目录不存在时自动创建
- 最多支持 5 个路径（同一文档挂到多个目录）
- 文档创建后自动挂载到 TOC 目录节点下

## 调完干嘛

- 检查 `results` 中 `failed` 数量
- 打开新文档链接确认内容正确

## 别干的事

- 不要传未经清洗的原始 body（可能含控制字符导致 JSON 解析失败）
- 不要并发复制同一篇文档到同一个目录
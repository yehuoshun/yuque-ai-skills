# yuque_upload_attachment

## API

`POST /api/upload/attach` — Web API（Cookie 认证，非 v2 OpenAPI）。

> 需要 `config.json` 中配置 `cookie` + `ctoken`。

## 什么时候调

- 上传图片/附件/视频到语雀 CDN
- 获取 CDN URL 后在文档中引用

## 调用

```bash
curl -s -X POST \
  -H "Cookie: $YUQUE_COOKIE" \
  -H "x-csrf-token: $YUQUE_CTOKEN" \
  -F "file=@/path/to/file.png" \
  "https://www.yuque.com/api/upload/attach?attachable_type=User&attachable_id=25689388&type=image&ctoken=$YUQUE_CTOKEN"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `file_path` | 必填，本地文件路径 |
| `type` | image / attachment / video，默认 attachment |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `url` | CDN 地址（`https://cdn.nlark.com/{filekey}`） |
| `filekey` | 文件 key |
| `extname` | 扩展名 |

## 调完干嘛

- 拿到 `url` 后嵌入文档正文（`![alt](url)` 或 Markdown 链接）
- 配合 `yuque_update_doc` 更新文档内容

## 别干的事

- 别超过大小限制：图片 20MB / 附件 500MB / 视频 500MB（专业会员）
- 别忘记配 cookie/ctoken
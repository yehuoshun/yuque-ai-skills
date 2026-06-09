# yuque_create_note

## API

`POST /api/v2/notes` — 完整文档见 `references/api/note_api.md`。

## 什么时候调

- 创建新小记

## 调用

```bash
curl -s -X POST -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"body":"今天学到的新东西"}' \
  "https://www.yuque.com/api/v2/notes"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `body` | 必填，小记内容（纯文本或 Markdown） |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `id` | 新小记 ID |
| `slug` | 路径 |
| `note_url` | 小记链接 |
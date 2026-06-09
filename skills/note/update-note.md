# yuque_update_note

## API

`PUT /api/v2/notes/{id}` — 完整文档见 `references/api/note_api.md`。

## 什么时候调

- 修改小记内容

## 调用

```bash
curl -s -X PUT -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"body":"更新后的内容"}' \
  "https://www.yuque.com/api/v2/notes/87204907"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `note_id` | 必填，小记 ID |
| `body` | 必填，新内容（纯文本或 Markdown） |

## 返回

返回更新后的小记详情。
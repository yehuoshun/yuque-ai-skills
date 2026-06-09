# yuque_list_notes

## API

`GET /api/v2/notes` — 完整文档见 `references/api/note_api.md`。

## 什么时候调

- 查看当前用户的小记列表
- 浏览置顶小记

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/notes?limit=20"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `status` | 0=正常 / 9=已删除 |
| `page` | 页码，默认 1 |
| `limit` | 每页数量，默认 20 |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `pin_notes` | 置顶小记数组 |
| `notes` | 普通小记数组 |
| `notes[].id` | 小记 ID |
| `notes[].slug` | 小记路径 |
| `notes[].content.abstract` | 内容摘要 |

完整字段见 `references/api/note_api.md`。

## 调完干嘛

- 展示小记列表
- 拿到 ID 后调 `yuque_get_note` 获取详情

## 别干的事

- 别期望返回完整正文——列表只返回摘要，详情用 `yuque_get_note`
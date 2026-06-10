# 获取小记详情

## API

`GET /api/v2/notes/{id}` — 完整文档见 `references/api/note_api.md`。

## 什么时候调

- 查看小记完整内容

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/notes/87204907"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `note_id` | 必填，小记 ID |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `id` | 小记 ID |
| `slug` | 路径 |
| `content.source` | 纯文本内容 |
| `content.html` | HTML 内容 |
| `word_count` | 字数 |
| `tags` | 标签 |
| `is_pinned` | 是否置顶 |

完整字段见 `references/api/note_api.md`。

## 调完干嘛

- 展示小记完整内容给用户

## 别干的事

- 别期望列表接口返回完整正文——列表只返回摘要
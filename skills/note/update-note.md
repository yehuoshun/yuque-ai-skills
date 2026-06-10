# 更新小记

## API

`PUT /api/v2/notes/{id}` — 完整文档见 `references/api/note_api.md`。

## 什么时候调

- 修改小记内容
- 删除/恢复小记（`status=9` 删除，`status=0` 恢复）

## 调用

```bash
# 更新内容
curl -s -X PUT -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"body":"更新后的内容"}' \
  "https://www.yuque.com/api/v2/notes/87204907"

# 删除（软删除）
curl -s -X PUT -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"status":9}' \
  "https://www.yuque.com/api/v2/notes/87204907"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `note_id` | 必填，小记 ID |
| `body` | 新内容（纯文本或 Markdown，不填则保持不变） |
| `status` | 0=正常 / 9=删除（不填则不变） |

## 返回

返回更新后的小记详情。

## 调完干嘛

- 告知用户更新结果
- 如需确认内容，调 `GET /api/v2/notes/:id`（获取小记详情）验证

## 别干的事

- 别用 `status=9` 当永久删除——这是软删除，小记仍可恢复
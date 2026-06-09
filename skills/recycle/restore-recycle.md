# yuque_restore_recycle

## API

`PUT /api/mine/recycles/{id}/restore` — Web API（Cookie 认证）。

## 什么时候调

- 恢复误删的文档/知识库

## 调用

```bash
curl -s -X PUT -H "Cookie: $YUQUE_COOKIE" \
  -H "x-csrf-token: $YUQUE_CTOKEN" \
  "https://www.yuque.com/api/mine/recycles/123/restore"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `recycle_id` | 必填，回收站项目 ID（从 `yuque_list_recycles` 获取） |

## 返回

```json
{"restored": true, "recycle_id": 123}
```

## 调完干嘛

- 告知用户恢复成功
- 确认文档已回到原知识库
# 彻底删除回收站项目

## API

`DELETE /api/mine/recycles/{id}` — Web API（Cookie 认证）。

> ⚠️ 不可恢复！

## 什么时候调

- 彻底清空回收站中的项目

## 调用

```bash
curl -s -X DELETE -H "Cookie: $YUQUE_COOKIE" \
  -H "x-csrf-token: $YUQUE_CTOKEN" \
  "https://www.yuque.com/api/mine/recycles/123"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `recycle_id` | 必填，回收站项目 ID |

## 返回

```json
{"destroyed": true, "recycle_id": 123}
```

## 调完干嘛

- 告知用户已彻底删除，不可恢复

## 别干的事

- 删之前先确认——不可恢复
- 别跳过回收站列表直接删——先调 `GET /api/mine/recycles`（列出回收站）确认 ID
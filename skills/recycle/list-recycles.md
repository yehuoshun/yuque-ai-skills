# yuque_list_recycles

## API

`GET /api/mine/recycles` — Web API（Cookie 认证，非 v2 OpenAPI）。

> 需要环境变量 `YUQUE_COOKIE` 和 `YUQUE_CTOKEN`。获取方式：浏览器登录 yuque.com → F12 → Application → Cookies → 复制 `_yuque_session` 和 `yuque_ctoken`。

## 什么时候调

- 查看回收站内容
- 确认被删除的文档/知识库

## 调用

```bash
curl -s -H "Cookie: $YUQUE_COOKIE" \
  -H "x-csrf-token: $YUQUE_CTOKEN" \
  "https://www.yuque.com/api/mine/recycles?offset=0&limit=50"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `offset` | 分页偏移，默认 0 |
| `limit` | 每页数量，≤100，默认 50 |
| `target_type` | 类型筛选：Doc / Note / Repo |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `total` | 总数 |
| `items[].id` | 回收站项目 ID |
| `items[].target_type` | Doc / Note / Repo |
| `items[].title` | 标题 |
| `items[].book` | 所属知识库信息 |

## 调完干嘛

- 找到要恢复的项目 → 调 `yuque_restore_recycle`
- 确认不需要的 → 调 `yuque_destroy_recycle`

## 别干的事

- 别忘记配 Cookie 环境变量——否则直接报错
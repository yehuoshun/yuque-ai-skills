# 团队汇总统计数据

## API

`GET /api/v2/groups/{login}/statistics` — 完整文档见 `references/api/statistic_api.md`。

> Token 需要 `statistic:read` 权限。返回团队维度汇总数据。

## 什么时候调

- 查看团队使用数据
- 生成团队运营报表

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/groups/uuctgr/statistics"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `login` | 必填，团队的 Login 或 ID |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `member_count` | 成员数 |
| `doc_count` | 文档数量 |
| `book_count` | 知识库总数 |
| `read_count` | 总阅读量 |
| `read_count_30` | 阅读量（30天） |
| `like_count` | 总点赞量 |
| `comment_count` | 总评论量 |
| `data_usage` | 流量使用量 |

完整字段见 `references/api/statistic_api.md`。

## 调完干嘛

- 展示团队数据概览
- 对比不同时间段数据

## 别干的事

- 别在 Token 没有 `statistic:read` 权限时调——会 401
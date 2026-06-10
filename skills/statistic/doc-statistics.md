# 团队文档统计数据

## API

`GET /api/v2/groups/{login}/statistics/docs` — 完整文档见 `references/api/statistic_api.md` → 文档统计。

> Token 需要 `statistic:read` 权限。支持按知识库/文档名过滤、时间范围、多字段排序。

## 什么时候调

- 查看团队热门文档排行
- 按阅读量/点赞量/评论量排序找热门文档
- 分析指定知识库的文档活跃度

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/groups/uuctgr/statistics/docs?range=30&sortField=read_count&sortOrder=desc&limit=10"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `login` | 必填，团队 Login 或 ID |
| `bookId` | 指定知识库 ID 过滤 |
| `name` | 文档名过滤 |
| `range` | 时间范围：0=全部 / 30=近30天 / 365=近一年（默认 0） |
| `page` | 页码，默认 1 |
| `limit` | 每页数量，≤20，默认 10 |
| `sortField` | 排序字段：content_updated_at / word_count / read_count / like_count / comment_count / created_at |
| `sortOrder` | desc（默认）/ asc |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `docs.doc_id` | 文档 ID |
| `docs.title` | 文档标题 |
| `docs.slug` | 文档路径 |
| `docs.book_id` | 所属知识库 ID |
| `docs.read_count` | 阅读量 |
| `docs.like_count` | 点赞量 |
| `docs.comment_count` | 评论量 |
| `docs.popularity_30` | 30天热度 |
| `total` | 总数量 |

完整字段见 `references/api/statistic_api.md`。

## 调完干嘛

- 展示热门文档排行
- 分析文档阅读趋势

## 别干的事

- 别传 `limit > 20`——API 限制 ≤20
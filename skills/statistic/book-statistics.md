# yuque_get_book_statistics

## API

`GET /api/v2/groups/{login}/statistics/books` — 完整文档见 `references/api/statistic_api.md` → 知识库统计。

> Token 需要 `statistic:read` 权限。支持名称过滤、时间范围、多字段排序。

## 什么时候调

- 查看团队知识库活跃度排行
- 按阅读量/点赞量/关注量排序找热门知识库

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/groups/uuctgr/statistics/books?range=30&sortField=read_count&sortOrder=desc&limit=10"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `login` | 必填，团队 Login 或 ID |
| `name` | 知识库名过滤 |
| `range` | 时间范围：0=全部 / 30=近30天 / 365=近一年（默认 0） |
| `page` | 页码，默认 1 |
| `limit` | 每页数量，≤20，默认 10 |
| `sortField` | 排序字段：content_updated_at_ms / word_count / post_count / read_count / like_count / watch_count / comment_count |
| `sortOrder` | desc（默认）/ asc |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `books.book_id` | 知识库 ID |
| `books.name` | 知识库名称 |
| `books.slug` | 知识库路径 |
| `books.read_count` | 阅读量 |
| `books.like_count` | 点赞量 |
| `books.watch_count` | 关注量 |
| `books.post_count` | 文档数 |
| `books.popularity_30` | 30天热度 |
| `total` | 总数量 |

完整字段见 `references/api/statistic_api.md`。

## 调完干嘛

- 展示热门知识库排行
- 分析知识库使用趋势

## 别干的事

- 别传 `limit > 20`——API 限制 ≤20
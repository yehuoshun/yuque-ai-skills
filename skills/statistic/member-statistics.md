# 团队成员统计数据

## API

`GET /api/v2/groups/{login}/statistics/members` — 完整文档见 `references/api/statistic_api.md` → 成员统计。

> Token 需要 `statistic:read` 权限。支持姓名过滤、时间范围、排序。

## 什么时候调

- 查看团队成员活跃度
- 按编辑/阅读/点赞排序找出活跃成员

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/groups/uuctgr/statistics/members?range=30&sortField=write_doc_count&sortOrder=desc&limit=10"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `login` | 必填，团队 Login 或 ID |
| `name` | 成员名过滤（模糊匹配） |
| `range` | 时间范围：0=全部 / 30=近30天 / 365=近一年（默认 0） |
| `page` | 页码，默认 1 |
| `limit` | 每页数量，≤20，默认 10 |
| `sortField` | 排序字段：write_doc_count / write_count / read_count / like_count |
| `sortOrder` | desc（默认）/ asc |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `members.user` | 成员名 |
| `members.write_doc_count` | 编辑文档数 |
| `members.write_count` | 编辑次数 |
| `members.read_count` | 阅读量 |
| `members.like_count` | 点赞量 |
| `total` | 总数量 |

各 count 字段有 `_30` / `_365` 变体对应近30天/近一年。

完整字段见 `references/api/statistic_api.md`。

## 调完干嘛

- 展示活跃成员排行
- 按时间维度对比成员贡献

## 别干的事

- 别传 `limit > 20`——API 限制 ≤20
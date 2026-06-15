# yuque_rss_fetch

> 端点：无（RSS 抓取写入）
> 说明：抓取 RSS/Atom Feed，解析后去重写入语雀知识库

## 参数

| 参数 | 类型 | 必填 | 默认 | 说明 |
|------|------|:--:|------|------|
| source | string | ✅ | - | 数据源 key，如 `cnblogs` |
| feed_type | string | ✅ | - | feed 类型，如 `sitehome`、`picked`、`user` |
| feed_params | object | ❌ | - | 模板参数，如 `{ username: "hsewr333" }` |
| target_repo | string | ✅ | - | 目标知识库 ID 或 namespace |
| max_items | number | ❌ | 10 | 最大抓取数，上限 50 |
| mode | string | ❌ | `append` | `append` / `update` / `dry_run` |
| dedup_field | string | ❌ | `title` | 去重字段：`title` / `link` / `guid` |
| title_prefix | string | ❌ | - | 文档标题前缀，如 `[博客园] ` |
| raw | boolean | ❌ | false | 返回原始 JSON |

## 响应

### dry_run 模式（预览不写入）

```json
{
  "mode": "dry_run",
  "source": "博客园 / 首页最新",
  "feed_url": "https://feed.cnblogs.com/blog/sitehome/rss",
  "feed_title": "博客园_首页",
  "total": 3,
  "entries": [
    {
      "title": "文章标题",
      "link": "https://www.cnblogs.com/xxx/p/123",
      "author": "作者名",
      "published": "2026-06-15T05:40:00Z"
    }
  ]
}
```

### append 模式（写入语雀）

```json
{
  "source": "博客园 / 首页最新",
  "feed_url": "https://feed.cnblogs.com/blog/sitehome/rss",
  "fetched": 10,
  "new": 3,
  "skipped": 7,
  "dedup": { "field": "title", "existing_docs": 42 },
  "results": [
    {
      "title": "文章标题",
      "link": "https://www.cnblogs.com/xxx/p/123",
      "doc_id": 12345,
      "status": "created"
    }
  ]
}
```

## 写入文档格式

每篇 RSS 条目 → 一篇语雀 Markdown 文档：

```markdown
> 来源：博客园 | 作者：狂师 | 2026-06-15 12:50

# 文章标题

文章摘要内容...

[原文链接](https://www.cnblogs.com/xxx/p/123)
```

## 使用场景

- **信息聚合**：定期抓取技术博客到语雀知识库
- **监控关注作者**：用 `user` feed + `feed_params` 追踪指定作者
- **先预览再写入**：先用 `dry_run` 看效果，确认后再 `append`
- **分类订阅**：按分类抓取，如只关注 .NET 分类

## 注意事项

- 去重依据目标知识库已有文档标题，首次运行前建议确认知识库为空或先 dry_run
- 去重最多检查 500 篇文档（5 页 x 100），超大知识库可能漏检
- 语雀 API 有频率限制，大量写入时注意 `max_items` 不要设太高
- 此 tool 不抓取全文，只存 RSS 摘要。如需全文请用爬虫 tool
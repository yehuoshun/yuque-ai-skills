# yuque_rss_fetch

> 端点：无（RSS 抓取写入）
> 说明：抓取 RSS/Atom Feed，语雀 KV slug 去重后写入知识库

## 参数

| 参数 | 类型 | 必填 | 默认 | 说明 |
|------|------|:--:|------|------|
| source | string | ✅ | - | 数据源 key，如 `cnblogs` |
| feed_type | string | ✅ | - | feed 类型，如 `sitehome`、`picked`、`user` |
| feed_params | object | ❌ | - | 模板参数，如 `{ username: "hsewr333" }` |
| target_repo | string | ❌ | config | RSS 目标知识库，不传则从 config.json `rss` 配置读取 |
| kv_repo | string | ❌ | config | KV 去重知识库，不传则从 config.json `kv` 配置读取 |
| max_items | number | ❌ | 10 | 最大抓取数，上限 50 |
| mode | string | ❌ | `append` | `append` / `dry_run` |
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
      "slug": "cnblogs-20538011",
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
  "target_repo": "123456",
  "kv_repo": "123456",
  "fetched": 10,
  "new": 3,
  "skipped": 7,
  "dedup": { "strategy": "yuque-kv-slug" },
  "results": [
    {
      "title": "文章标题",
      "link": "https://www.cnblogs.com/xxx/p/123",
      "slug": "cnblogs-20538011",
      "doc_id": 12345,
      "status": "created"
    }
  ]
}
```

## 去重机制

**语雀 KV slug 方案**：每篇文章以 `{source}-{site_article_id}` 作为文档 slug（如 `cnblogs-20538011`），通过 `GET /repos/{kv_repo}/docs/{slug}` 判断是否已存在。O(1) 单次 API 调用，无文档数量上限。

博客园链接 `https://www.cnblogs.com/xxx/p/20538011` → slug `cnblogs-20538011`

其他数据源 fallback 到 `md5(link).slice(0, 12)`。

## 写入文档格式

每篇 RSS 条目 → 一篇语雀 Markdown 文档：

```markdown
> 来源：博客园 | 作者：狂师 | 2026-06-15 12:50

# 文章标题

文章摘要内容...

[原文链接](https://www.cnblogs.com/xxx/p/123)
```

文档 `description` 字段存储原文链接，`slug` 存储去重标识。

## 配置

### config.json

```json
{
  "rss": {
    "default_repo": { "book_id": "123456", "namespace": "my-group/default" },
    "cnblogs": { "book_id": "123456", "enable_kv": true }
  },
  "kv": {
    "default_repo": { "book_id": "789012", "namespace": "my-group/kv" },
    "cnblogs": { "book_id": "789012" }
  }
}
```

- `rss`：RSS 文章写入的目标知识库
- `kv`：去重 slug 查询的知识库（可与 rss 相同）
- `enable_kv`：false 时跳过去重，全量写入
- 不传 `target_repo` / `kv_repo` 参数时自动从配置读取

## 使用场景

- **信息聚合**：定期抓取技术博客到语雀知识库
- **监控关注作者**：用 `user` feed + `feed_params` 追踪指定作者
- **先预览再写入**：先用 `dry_run` 看效果，确认后再 `append`
- **分类订阅**：按分类抓取，如只关注 .NET 分类

## 注意事项

- 去重基于语雀 KV slug 方案，不依赖扫全库，无文档数量上限
- 语雀 API 有频率限制，大量写入时注意 `max_items` 不要设太高
- 此 tool 不抓取全文，只存 RSS 摘要。如需全文请用爬虫 tool
- 知识库文档上限 5000 篇，超限时语雀 API 会报错
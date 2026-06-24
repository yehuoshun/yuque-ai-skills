# yuque_rss_fetch

> 端点：无（RSS 抓取写入）
> 说明：抓取 RSS/Atom Feed，语雀 KV 去重后写入知识库

## 参数

| 参数 | 类型 | 必填 | 默认 | 说明 |
|------|------|:--:|------|------|
| source | string | ✅ | - | 数据源 key，对应 config.json `rss.sources` 中的 key |
| feed_type | string | ✅ | - | feed 类型，对应 `rss.sources.{source}.feeds` 中的 key |
| feed_params | object | ❌ | - | 模板参数，如 `{ username: "xxx" }` |
| target_repo | string | ❌ | config | 目标知识库 ID，不传则从 config.json `rss.namespaces.{source}.book_id` 读取 |
| kv_namespace | string | ❌ | source | KV 去重命名空间，默认等于 source |
| max_items | number | ❌ | 10 | 最大抓取数，上限 50 |
| mode | string | ❌ | `append` | `append` / `dry_run` |
| title_prefix | string | ❌ | - | 文档标题前缀，如 `[SourceName] ` |
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
      "link": "https://example.com/post/123",
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
  "target_repo": 0,
  "kv_namespace": "cnblogs",
  "fetched": 10,
  "new": 3,
  "skipped": 7,
  "dedup": { "strategy": "yuque-kv-json-map" },
  "results": [
    {
      "title": "文章标题",
      "link": "https://example.com/post/123",
      "slug": "cnblogs-20538011",
      "doc_id": 12345,
      "status": "created"
    }
  ]
}
```

## 去重机制

**语雀 KV JSON Map 方案**：每篇文章以 slug 为 key 存入 KV 文档（JSON map），O(1) 查找。

slug 生成优先级：
1. `config.json` 中 `rss.sources.{source}.slug_pattern` 正则提取站点文章 ID → `{source}-{id}`
2. fallback：`md5(link).slice(0, 12)`

示例：`slug_pattern: "/p/(\\d+)"` + 链接 `https://example.com/p/20538011` → slug `mysource-20538011`

## 写入文档格式

每篇 RSS 条目 → 一篇语雀 Markdown 文档：

```markdown
> 来源：SourceName | 作者：Author | 2026-06-15 12:50

# 文章标题

文章摘要内容...

[原文链接](https://example.com/post/123)
```

文档 `description` 字段存储原文链接，`slug` 存储去重标识。

## 配置

### config.json

```json
{
  "rss": {
    "enabled": true,
    "sources": {
      "cnblogs": {
        "name": "博客园",
        "slug_pattern": "/p/(\\d+)",
        "feeds": {
          "sitehome": { "label": "首页", "url": "https://feed.cnblogs.com/blog/sitehome/rss" }
        }
      }
    },
    "namespaces": {
      "cnblogs": {
        "book_id": [0],
        "kv_slugs": ["0/0"],
        "schedule_slugs": []
      }
    }
  },
  "kv": { "enabled": true }
}
```

- `rss.sources`：RSS 源定义（name、feeds、slug_pattern）。新增源只需在此追加。
- `rss.namespaces`：每个源的运行时配置（目标知识库、KV 分片、定时策略）
- `kv.enabled`：控制去重开关

## 使用场景

- **信息聚合**：定期抓取技术博客到语雀知识库
- **监控关注作者**：用 `user` feed + `feed_params` 追踪指定作者
- **先预览再写入**：先用 `dry_run` 看效果，确认后再 `append`
- **分类订阅**：按分类抓取，如只关注 .NET 分类

## 注意事项

- 去重基于语雀 KV JSON Map 方案，增量分片，单文档上限 250KB
- 语雀 API 有频率限制，大量写入时注意 `max_items` 不要设太高
- 此 tool 不抓取全文，只存 RSS 摘要。如需全文请用爬虫 tool
- 知识库文档上限 5000 篇，超限时自动扩容
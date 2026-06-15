# RSS API 参考

> RSS 抓取域，不直接调用语雀 API 端点，而是抓取外部 RSS/Atom Feed 后写入语雀。

## 端点

### yuque_rss_list_sources

```
无（RSS 数据源列表）
```

列出所有可用 RSS 数据源及 feed 类型。

**无参数**。

**响应示例**：
```json
{
  "sources": [
    {
      "key": "cnblogs",
      "name": "博客园",
      "description": "开发者的网上家园，技术博客聚合平台",
      "feeds": [
        {
          "key": "sitehome",
          "label": "首页最新",
          "has_template": false,
          "params": []
        },
        {
          "key": "user",
          "label": "用户博客",
          "has_template": true,
          "params": [
            { "name": "username", "type": "string", "required": true }
          ]
        }
      ]
    }
  ]
}
```

### yuque_rss_fetch

```
无（RSS 抓取写入）
```

抓取 RSS/Atom Feed，解析条目，去重后写入目标知识库。

**参数**：

| 参数 | 类型 | 必填 | 说明 |
|------|------|:--:|------|
| source | string | ✅ | 数据源 key，如 `cnblogs` |
| feed_type | string | ✅ | feed 类型，如 `sitehome`、`picked` |
| feed_params | object | ❌ | 模板参数，如 `{ username: "hsewr333" }` |
| target_repo | string | ✅ | 目标知识库 ID 或 namespace |
| max_items | number | ❌ | 最大抓取数，默认 10，上限 50 |
| mode | string | ❌ | `append`（默认）/ `update` / `dry_run` |
| dedup_field | string | ❌ | 去重字段：`title`（默认）/ `link` / `guid` |
| title_prefix | string | ❌ | 文档标题前缀，如 `[博客园] ` |

**响应示例**（dry_run）：
```json
{
  "mode": "dry_run",
  "source": "博客园 / 首页最新",
  "total": 3,
  "entries": [
    { "title": "...", "link": "https://...", "author": "...", "published": "..." }
  ]
}
```

**响应示例**（append）：
```json
{
  "source": "博客园 / 首页最新",
  "fetched": 10,
  "new": 3,
  "skipped": 7,
  "dedup": { "field": "title", "existing_docs": 42 },
  "results": [
    { "title": "...", "link": "...", "doc_id": 12345, "status": "created" }
  ]
}
```

## 使用场景

1. **信息聚合**：定期抓取技术博客到语雀知识库，建立个人技术情报站
2. **监控关注作者**：用 `user` feed + 定时任务，自动追踪指定作者的更新
3. **分类订阅**：按技术分类抓取，如只关注 `.NET` 分类的文章

## 扩展新数据源

编辑 `server/src/rss/sources.ts`，追加条目即可，无需改 tool 代码：

```ts
"new_source": {
  name: "新数据源",
  feeds: {
    main: { label: "主 feed", url: "https://example.com/rss" },
    user: {
      label: "用户",
      url_template: "https://example.com/user/{username}/rss",
      params_schema: {
        username: { type: "string", description: "用户名", required: true },
      },
    },
  },
},
```
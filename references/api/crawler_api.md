# Crawler API 参考

爬虫域工具，用于网页抓取、内容提取和自动写入语雀。

## 工具列表

| 工具 | 说明 |
|------|------|
| `yuque_crawl_fetch` | HTTP GET 抓取网页原始 HTML |
| `yuque_crawl_extract` | CSS 选择器从 HTML 提取内容 |
| `yuque_crawl_save` | 抓取→提取→去重→写入语雀一站式管道 |

## 配置

```json
{
  "crawler": {
    "enabled": true,
    "default_repo": { "id": 80197497 },
    "sources": {
      "cnblogs": { "id": 80197497 }
    }
  }
}
```

### 目标知识库解析优先级

1. `target_repo` 参数（直接指定）
2. `crawler.sources.{source}`（按来源匹配）
3. `crawler.default_repo`（默认配置）

## 去重

- 依赖 `kv.enabled = true`
- slug = URL 的 md5 前 12 位
- 通过 `GET /repos/{kv_repo}/docs/{slug}` 检查是否存在
- 已存在则跳过，不重复写入

## 使用模式

### 模式一：一站式（推荐）

```json
// 直接调用 yuque_crawl_save，一次完成
{
  "url": "https://example.com/article/123",
  "source": "cnblogs",
  "content_selector": ".post-body"
}
```

### 模式二：分步控制

```json
// 1. 抓取
yuque_crawl_fetch → { "url": "..." } → HTML

// 2. Agent 分析 HTML，确定选择器

// 3. 提取
yuque_crawl_extract → { "html": "...", "selector": "..." } → 结构化数据

// 4. Agent 决定如何处理提取结果（写入语雀、过滤、转换等）
```

## 限制

- 仅支持静态 HTML 页面，不处理 JS 渲染（SPA）
- CSS 选择器不支持伪类、属性值匹配、兄弟选择器
- 无 JavaScript 执行环境

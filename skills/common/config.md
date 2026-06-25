# 配置

所有工具共用此配置结构。

```json
{
  "token": "语雀 API Token",
  "api_base": "https://www.yuque.com/api/v2",
  "cookie": "可选，回收站/上传/mine 功能需要",
  "ctoken": "可选，从 Cookie 中提取",
  "kv": { "enabled": true },
  "rss": {
    "enabled": true,
    "namespaces": {
      "cnblogs": {
        "book_id": 0,
        "kv_slugs": [],
        "schedule_slugs": []
      }
    }
  },
  "crawler": {
    "enabled": true,
    "namespaces": {
      "my-source": {
        "book_id": 0,
        "kv_slugs": [],
        "schedule_slugs": []
      }
    }
  }
}
```

## 配置项说明

| 字段 | 说明 |
|------|------|
| `token` | 语雀 API Token，必填 |
| `api_base` | API 基地址，默认 `https://www.yuque.com/api/v2` |
| `cookie` | Cookie 字符串，回收站/上传/mine/web-search 功能需要 |
| `ctoken` | CSRF Token，从 Cookie 中提取，配合 cookie 使用 |
| `kv` | KV 存储配置，`enabled: true` 启用 |
| `rss` | RSS 抓取配置，按 namespace 分组 |
| `crawler` | 网页爬虫配置，按 namespace 分组 |

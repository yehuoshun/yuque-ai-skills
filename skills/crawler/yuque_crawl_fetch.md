# yuque_crawl_fetch

抓取网页原始 HTML，不做任何解析/提取。

## 端点

无（工具内部 HTTP 请求，不调语雀 API）

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|:--:|------|
| `url` | string | ✅ | 目标 URL |
| `headers` | string | ❌ | 自定义请求头，JSON 字符串 |
| `timeout` | number | ❌ | 超时 ms（默认 15000，最大 30000） |
| `raw` | boolean | ❌ | 返回原始完整 JSON |

## 返回

```json
{
  "url": "最终 URL（跟随重定向后）",
  "status": 200,
  "headers": { "content-type": "text/html" },
  "body": "<html>...</html>",
  "bodySize": 12345,
  "elapsed": 234
}
```

## 使用场景

- Agent 需要分析页面结构，自行决定如何提取
- 需要处理复杂选择器（`:nth-child` 等 extract 不支持的情况）
- 需要检查响应头（编码、重定向等）

## 注意事项

- 默认 User-Agent: `Mozilla/5.0 (compatible; YuqueCrawler/1.0)`
- 自动跟随重定向
- 超时后返回 `TIMEOUT` 错误
- 不处理 JS 渲染页面（SPA），仅静态 HTML

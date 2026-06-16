# yuque_crawl_extract

从 HTML 中用 CSS 选择器提取内容。

## 端点

无（纯字符串解析，不调外部 API）

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|:--:|------|
| `html` | string | ✅ | 原始 HTML 字符串 |
| `selector` | string | ✅ | CSS 选择器 |
| `attr` | string | ❌ | 提取指定属性值（如 `href`、`src`），不传则提取文本 |
| `limit` | number | ❌ | 最大返回数（默认 50，最大 200） |
| `raw` | boolean | ❌ | 返回原始完整 JSON |

## 支持的选择器

| 类型 | 示例 | 说明 |
|------|------|------|
| 标签 | `div`, `p`, `a` | 匹配标签名 |
| 类 | `.content`, `.article-title` | 匹配 class |
| ID | `#main` | 匹配 id |
| 标签+类 | `div.content`, `a.link` | 组合匹配 |
| 属性 | `[href]`, `[data-id]` | 匹配有该属性的元素 |
| 嵌套 | `div.content a.link` | 父级+子级组合 |

## 不支持

- 伪类（`:nth-child`、`:first-child`）
- 属性值匹配（`[attr=val]`）
- 兄弟选择器（`+`、`~`）
- 复杂场景请用 `yuque_crawl_fetch` 拿 HTML 后 Agent 自行解析

## 返回

```json
{
  "selector": "div.content a.link",
  "count": 5,
  "items": [
    {
      "text": "链接文本",
      "html": "<a class=\"link\" href=\"/post/123\">链接文本</a>",
      "attrs": { "class": "link", "href": "/post/123" }
    }
  ]
}
```

传 `attr` 参数时只返回属性值数组：
```json
{
  "selector": "a.link",
  "attr": "href",
  "count": 5,
  "values": ["/post/123", "/post/456", ...]
}
```

## 注意事项

- 零外部依赖，纯正则+字符串解析
- 大 HTML（>1MB）可能较慢，建议 Agent 先用 `yuque_crawl_fetch` 确认页面大小

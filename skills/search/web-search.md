# Cookie 态 Web 搜索

## API

`GET /api/zsearch` — Web API（Cookie 认证），完整文档见 `references/api/search_api.md`。

## 什么时候调

- 需要搜索后直接拿到完整文档对象（省去二次 `get_doc` 调用）
- 需要精确的搜索结果总数
- 需要搜用户（`type=user`）
- 需要高亮摘要（`abstract` 字段含 `<em>` 标签）

## 与 `yuque_search` 的区别

| | `yuque_search` (API v2) | `yuque_web_search` (Cookie) |
|---|---|---|
| 认证 | Token | Cookie + ctoken |
| 返回文档 | 摘要 | 完整 `_record`（等同于 get_doc） |
| 总数 | ❌ | ✅ `totalHits` |
| 高亮摘要 | ❌ | ✅ `abstract` |
| 搜用户 | ❌ | ✅ `type=user` |
| 搜知识库 | ✅ `type=repo` | ✅ `type=book` |
| scope 过滤 | ✅ | ✅ |

## 调用

```bash
curl -s 'https://www.yuque.com/api/zsearch?q={关键词}&type=content&scope=%2F&p=1&sence=searchPage' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'x-csrf-token: {ctoken}' \
  -b '{cookie}'
```

## 高级搜索语法

`q` 参数支持以下高级语法（来源：语雀官方《高级搜索语法》）：

### 精确短语
用英文双引号 `"..."` 包裹含空格的词组，禁止分词。
```
"知识 大会"     → 匹配"知识大会"，不做分词
```

### 标题限定
`in:title` 将搜索范围限定在标题中。
```
语雀是什么 in:title     → 标题含"语雀是什么"
```

### 布尔运算符
`NOT`、`AND`、`OR` 仅用于字符串关键词。
```
帮助文档 NOT 桌面端      → 含"帮助文档"但不含"桌面端"
帮助文档 AND 桌面端      → 同时含两者
帮助文档 OR 桌面端       → 含任一
```

### 日期过滤
按更新时间过滤，格式 `YYYY-MM-DD`。
```
帮助 updated:>2021-01-01              → 2021-01-01 之后
帮助 updated:2021-01-01..2021-03-01    → 日期范围内
```

### 范围限定
`url:` 限定搜索特定团队、知识库或用户。
```
搜索 url:https://www.yuque.com/yuque              → 在"语雀"团队中
搜索 url:https://www.yuque.com/yuque/changelog     → 在指定知识库中
前端 url:https://www.yuque.com/zenany              → 在指定用户中
搜索 url:yuque                                     → 用团队ID
about yuque url:yuque/support_en                   → 团队ID/知识库slug
```

### 可见性过滤
```
搜索 is:related     → 仅"与我相关"
搜索 is:public      → 仅公开内容
```

> ⚠️ `is:related` 和 `url:` 同时使用时，`url:` 会被忽略。

## 参数要点

| 参数 | 说明 |
|------|------|
| `q` | 必填，搜索关键词 |
| `type` | `content`（文档）/ `book`（知识库）/ `user`（用户），默认 `content` |
| `scope` | 搜索范围，`/` 全局，`/group_slug` 团队，`/group_slug/book_slug` 知识库 |
| `p` | 页码，1-based，默认 1 |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `meta.total` | 精确结果总数 |
| `meta.num_hits` | 当前页命中数 |
| `data[].title` | 文档标题 |
| `data[].abstract` | 高亮摘要（含 `<em>` 标签） |
| `data[].abstract_html` | 原始高亮 HTML |
| `data[].book_name` | 所属知识库名 |
| `data[].group_name` | 所属团队名 |
| `data[].doc` | 精简版文档对象（id/slug/title/format/word_count/book_id/created_at/updated_at 等） |

## 调完干嘛

- 有结果 → 直接用 `doc` 字段，无需二次 `get_doc`
- 需要翻页 → 调 `p=2`
- 搜用户 → `type=user`

## ⚠️ 注意事项

- **QPS 未知，不要并发调用**。Cookie 态接口限流策略不透明，保守单次请求
- 需要 `cookie` + `ctoken` 配置，过期需重新抓取
- scope 需要 URL 编码（`/` → `%2F`）

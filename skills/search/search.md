# 通用搜索

## API

`GET /api/v2/search` — 完整文档见 `references/api/search_api.md`。

## 什么时候调

- 用户说「在语雀搜一下」「找文档」「有没有 XX 相关的」
- 需要跨知识库全文搜索时
- 注意：标题搜索优先用 `GET /api/v2/repos/{book_id}/docs` 按标题匹配，search 是全文搜索

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/search?q={关键词}&type=doc&scope={scope}&page=1"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `q` | 必填，≤200 字符 |
| `type` | 必填，`doc` 搜文档 / `repo` 搜知识库 |
| `scope` | 搜团队：`{group}`；搜知识库内：`{group}/{book_slug}` |
| `page` | 1-100，每页固定 20 条 |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `meta.total` | 结果总数 |
| `meta.pageNo` | 当前页码 |
| `data[].title` | 标题（含 `<em>` 高亮） |
| `data[].summary` | 摘要（含 `<em>` 高亮） |
| `data[].target.book_id` | 所属知识库 ID |
| `data[].target.slug` | 文档 slug |
| `data[].target.book.namespace` | 知识库 namespace |

## 调完干嘛

- 有结果 → 展示标题+摘要，让用户选
- 无结果 → 告知用户，建议换关键词或扩大 scope
- 翻页 → 调 `page=2`

## 别干的事

- 别用 `offset` 参数（已废弃，同 page）
- 别传 book_id 做 scope（只支持 namespace）
- 别用符号/纯数字做关键词（`[]` `-` `_` 匹配极差，受语雀分词影响）
- 别用非中英文关键词（语雀分词只支持中文/英文/中英混合）
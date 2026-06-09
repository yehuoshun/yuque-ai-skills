# yuque_get_doc

## API

`GET /api/v2/repos/docs/{id}` — 完整文档见 `references/api/doc_api.md` → 获取文档详情。

> `id` 支持文档数字 ID。slug 需配合 book 上下文（`repos/:book_id/docs/:slug`），建议先用 `yuque_list_docs` 拿 ID。

## 什么时候调

- 读取文档完整内容（正文、格式、元信息）
- 查看文档详情（创建者、状态、字数等）

## 调用

```bash
# 用文档 ID
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/repos/docs/273214401"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `id` | 必填，文档 ID（数字） |
| `page_size` | 数据表分页大小，1-200，默认 100 |
| `page` | 数据表页码，≥1，默认 1 |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `id` | 文档 ID |
| `slug` | 路径 |
| `title` | 标题 |
| `body` | Markdown 正文 |
| `body_html` | HTML 格式正文 |
| `body_lake` | Lake 格式正文 |
| `format` | 内容格式：markdown/lake/html |
| `type` | 文档类型：Doc/Sheet/Thread/Board/Table |
| `book_id` | 所属知识库 ID |
| `creator.name` | 创建者昵称 |

完整字段见 `references/api/doc_api.md`。

## 调完干嘛

- 拿到 `body` 即可展示/分析文档内容
- 用返回的 `slug` + `book_id` 去更新或删除文档

## 别干的事

- 别直接用 slug 当 `id` 传——`repos/docs/:slug` 会 404，需要 book 上下文
- 大量读取时注意 429 限流
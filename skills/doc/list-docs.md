# yuque_list_docs

## API

`GET /api/v2/repos/{book_id}/docs` — 完整文档见 `references/api/doc_api.md`。

> 等价于 `GET /api/v2/repos/{group_login}/{book_slug}/docs`，`book_id` 参数支持两种传法。

## 什么时候调

- 查看知识库下有哪些文档
- 需要获取文档 slug 去做后续操作（读/写/删）
- 分页浏览知识库目录

## 调用

```bash
# 用知识库 ID
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/repos/123456/docs?offset=0&limit=100"

# 用 namespace
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/repos/yehuoshun/gi49zs/docs?offset=0&limit=100"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `book_id` | 必填，知识库 ID 或 namespace（`group/book_slug`） |
| `offset` | 分页偏移，默认 0 |
| `limit` | 每页数量，≤100，默认 100。⚠️ 超过 100 时 `optional_properties` 失效 |
| `optional_properties` | 额外字段，逗号分隔。`hits`（阅读数）、`tags`（标签）、`latest_version_id`（最新版本号） |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `meta.total` | 文档总数 |
| `data[].id` | 文档 ID |
| `data[].slug` | 文档路径，后续操作需要 |
| `data[].title` | 标题 |
| `data[].type` | 类型：Doc/Sheet/Thread/Board/Table |
| `data[].status` | 0=草稿 / 1=发布 |
| `data[].book_id` | 所属知识库 ID |
| `data[].book.namespace` | 知识库完整路径 |

完整字段见 `references/api/doc_api.md`。

## 调完干嘛

- 拿到文档列表后，用 `slug` 去读/更新/删除具体文档
- 翻页时调 `offset`，配合 `meta.total` 判断是否还有下一页

## 别干的事

- 别传 `limit > 100`——会触发 `optional_properties` 失效
- 别期望按标题搜索——此端点只能列表，搜索用 `yuque_search`
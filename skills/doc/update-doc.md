# 更新文档

## API

`PUT /api/v2/repos/{book_id}/docs/{id}` — 完整文档见 `references/api/doc_api.md` → 更新文档。

> `book_id` 支持 ID 或 namespace，`id` 支持文档 ID 或 slug。所有 body 参数可选，只传需要更新的字段。

## 什么时候调

- 修改文档标题、正文、路径
- 批量更新文档内容
- 修改文档公开性

## 调用

```bash
curl -s -X PUT -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"title":"新标题","body":"新内容"}' \
  "https://www.yuque.com/api/v2/repos/67048677/docs/273214401"
```

## 参数要点

| 参数 | 位置 | 类型 | 说明 |
|------|------|------|------|
| `book_id` | path | string | 知识库 ID 或 namespace（必填） |
| `id` | path | string | 文档 ID 或 slug（必填） |
| `title` | body | string | 标题 |
| `slug` | body | string | 文档路径 |
| `format` | body | string | 内容格式：markdown / html / lake |
| `body` | body | string | 正文内容 |
| `public` | body | int | 0=私密 / 1=公开 / 2=企业内公开 |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `id` | 文档 ID |
| `title` | 更新后标题 |
| `body` | 更新后正文 |
| `updated_at` | 更新时间 |

完整字段见 `references/api/doc_api.md`。

## 调完干嘛

- 告知用户更新成功
- 如需确认内容，调 `GET /api/v2/repos/docs/:id`（获取文档详情） 验证

## 别干的事

- 别传空 body——至少传一个要更新的字段
- 别改 slug 后不更新引用——其他地方可能依赖旧 slug
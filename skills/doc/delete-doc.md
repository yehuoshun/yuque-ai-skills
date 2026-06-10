# 删除文档

## API

`DELETE /api/v2/repos/{book_id}/docs/{id}` — 完整文档见 `references/api/doc_api.md` → 删除文档。

> `book_id` 支持 ID 或 namespace，`id` 支持文档 ID 或 slug。删除后移入回收站。

## 什么时候调

- 删除不需要的文档
- 用户明确说「删掉 XX 文档」

## 调用

```bash
curl -s -X DELETE -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/repos/67048677/docs/273214401"
```

## 参数要点

| 参数 | 位置 | 类型 | 说明 |
|------|------|------|------|
| `book_id` | path | string | 知识库 ID 或 namespace（必填） |
| `id` | path | string | 文档 ID 或 slug（必填） |

## 返回

返回被删除的文档详情（V2DocDetail）。

## 调完干嘛

- 告知用户删除成功
- 文档只是移入回收站，可从回收站恢复

## 别干的事

- 删之前先确认——数据无价
- 批量删时注意限流
# yuque_delete_repo

## API

`DELETE /api/v2/repos/{book_id}` — 完整文档见 `references/api/repo_api.md` → 删除知识库。

> ⚠️ 不可恢复！删除前务必确认。

## 什么时候调

- 删除不需要的知识库
- 用户明确说「删掉 XX 知识库」

## 调用

```bash
curl -s -X DELETE -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/repos/67048677"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `book_id` | 必填，知识库 ID 或 namespace |

## 返回

返回被删除的知识库详情（V2Book）。

## 调完干嘛

- 告知用户删除成功
- 确认知识库已不存在（调 `yuque_get_repo` 应返回 404）

## 别干的事

- 删之前先确认——不可恢复
- 别在生产环境随意测试
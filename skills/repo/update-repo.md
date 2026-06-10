# 更新知识库

## API

`PUT /api/v2/repos/{book_id}` — 完整文档见 `references/api/repo_api.md` → 更新知识库。

> `book_id` 支持 ID 或 namespace。所有 body 参数可选，只传需要更新的字段。`toc` 字段可批量更新目录。

## 什么时候调

- 修改知识库名称、路径、简介
- 修改公开性
- 批量更新目录结构（通过 `toc` 字段）

## 调用

```bash
# 修改名称
curl -s -X PUT -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name":"新名称"}' \
  "https://www.yuque.com/api/v2/repos/67048677"

# 批量更新目录
curl -s -X PUT -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"toc":"- [新手指引]()\n - [快速开始](quickstart)\n- [API 参考]()\n - [用户](user-api)"}' \
  "https://www.yuque.com/api/v2/repos/67048677"
```

## 参数要点

| 参数 | 位置 | 类型 | 说明 |
|------|------|------|------|
| `book_id` | path | string | 知识库 ID 或 namespace（必填） |
| `name` | body | string | 名称 |
| `slug` | body | string | 路径 |
| `description` | body | string | 简介 |
| `public` | body | int | 0=私密 / 1=公开 / 2=企业内公开 |
| `toc` | body | string | 目录（Markdown 格式：`[名称](文档路径)`） |

## 返回

返回更新后的 V2Book。

## 调完干嘛

- 告知用户更新结果
- 用 `toc` 批量更新目录后，调 `GET /api/v2/repos/:book_id/toc`（获取目录） 确认

## 别干的事

- 别用 `toc` 做增量更新——它是全量替换
- 别传空 body——至少传一个要更新的字段
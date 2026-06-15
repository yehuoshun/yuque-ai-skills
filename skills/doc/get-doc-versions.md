# 获取文档历史版本列表

## API

`GET /api/v2/doc_versions?doc_id={doc_id}` — 完整文档见 `references/api/doc_api.md` → 文档历史版本。

## 什么时候调

- 查看文档修改历史
- 回溯文档内容变更

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/doc_versions?doc_id=273214401"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `doc_id` | 必填，文档 ID |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `id` | 版本 ID |
| `doc_id` | 所属文档 ID |
| `title` | 该版本时的标题 |
| `slug` | 该版本时的路径 |
| `user.name` | 发版人昵称 |
| `created_at` | 版本创建时间 |

返回最近 100 个已发布版本，按时间倒序。

完整字段见 `references/api/doc_api.md`。

## 调完干嘛

- 展示版本列表（版本号 + 时间 + 发版人）
- 配合版本详情接口查看具体内容差异

## 别干的事

- 别期望返回所有版本——最多 100 个
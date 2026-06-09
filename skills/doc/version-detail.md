# yuque_get_doc_version_detail

## API

`GET /api/v2/doc_versions/{id}` — 完整文档见 `references/api/doc_api.md` → 版本详情。

## 什么时候调

- 查看某个历史版本的具体内容
- 对比版本差异（diff 字段）
- 回溯文档修改历史

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/doc_versions/3910723645"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `id` | 必填，版本 ID（从 `yuque_get_doc_versions` 获取） |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `id` | 版本 ID |
| `doc_id` | 所属文档 ID |
| `title` | 该版本标题 |
| `format` | 内容格式 |
| `body` | Markdown 正文 |
| `body_html` | HTML 正文 |
| `body_asl` | Lake 格式正文 |
| `diff` | 版本差异 |
| `user.name` | 发版人 |

完整字段见 `references/api/doc_api.md`。

## 调完干嘛

- 展示版本内容或 diff
- 配合 `yuque_get_doc_versions` 做版本对比

## 别干的事

- 别期望 diff 字段一定有值——语雀不一定返回 diff
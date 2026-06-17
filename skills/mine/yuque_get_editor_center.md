# yuque_get_editor_center

获取个人编辑中心全景数据。

## 端点

`GET /api/mine/editor_center`

## 认证

Cookie 认证（需要 `cookie` + `ctoken`）。

## 返回字段

### overview（概览）

| 字段 | 说明 |
|------|------|
| `books.all` | 知识库总数 |
| `books.last_30d` | 近 30 天新增知识库 |
| `books.last_365d` | 近 365 天新增知识库 |
| `docs.all` | 文档总数 |
| `docs.last_30d` | 近 30 天新增文档 |
| `docs.last_365d` | 近 365 天新增文档 |
| `public_docs.all` | 公开文档数 |
| `notes.all` | 小记总数 |
| `words.all` | 总字数 |
| `selections` | 精选数 |
| `days_since_join` | 加入语雀天数 |

### editing（编辑）

| 字段 | 说明 |
|------|------|
| `edit_times.all` | 总编辑次数 |
| `edit_times.last_30d` | 近 30 天编辑次数 |
| `edit_days.all` | 总活跃天数 |
| `edit_doc_count.all` | 编辑过的文档总数 |

### engagement（互动）

| 字段 | 说明 |
|------|------|
| `liked.all` | 被点赞总数 |
| `public_likes.all` | 公开文档获赞总数 |
| `interactive_users` | 互动最多的用户列表 |

### max_word_book（最多字知识库）

| 字段 | 说明 |
|------|------|
| `name` | 知识库名称 |
| `items_count` | 文档数 |

## 使用示例

```json
mcporter call "yuque-mcp.yuque_get_editor_center"
```

## 注意事项

- 需要 Cookie 登录态，确保 `config.json` 中 `cookie` 和 `ctoken` 有效
- 返回的是个人维度聚合数据，非单个知识库数据
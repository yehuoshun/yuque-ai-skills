# 创建文档

## API

`POST /api/v2/repos/{book_id}/docs` — 完整文档见 `references/api/doc_api.md` → 创建文档。

> `book_id` 支持数字 ID 或 namespace 格式。✅ 创建后自动追加到知识库目录末尾。

## 什么时候调

- 在知识库中新建文档
- 批量导入/迁移内容到语雀

## 调用

```bash
curl -s -X POST -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"title":"新文档","body":"# Hello\n正文内容"}' \
  "https://www.yuque.com/api/v2/repos/67048677/docs"
```

## 参数要点

| 参数 | 位置 | 类型 | 说明 |
|------|------|------|------|
| `book_id` | path | string | 知识库 ID 或 namespace（必填） |
| `title` | body | string | 标题，默认「无标题」 |
| `slug` | body | string | 文档路径，不填自动生成 |
| `format` | body | string | 内容格式：markdown（默认）/ html / lake |
| `body` | body | string | 正文内容（必填） |
| `public` | body | int | 0=私密 / 1=公开 / 2=企业内公开，默认继承知识库 |
| `parent_uuid` | body | string | 父节点 UUID，文档挂到指定 TITLE 节点下，不传则挂根目录 |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `id` | 新文档 ID |
| `slug` | 文档路径，后续操作需要 |
| `title` | 标题 |
| `book_id` | 所属知识库 |

完整字段见 `references/api/doc_api.md`。

## 调完干嘛

- 告知用户创建成功（文档标题 + 链接）
- 已自动追加到目录末尾，无需手动操作

## 别干的事

- 别用 `format=lake` 除非明确需要语雀私有格式
- 如果自动追加目录失败，工具会返回警告信息，届时手动调 `yuque_update_toc` 补充
# 获取知识库详情

## API

`GET /api/v2/repos/{book_id}` — 完整文档见 `references/api/repo_api.md` → 获取知识库详情。

> `book_id` 支持数字 ID 或 namespace。返回含 `toc_yml` 的完整信息。

## 什么时候调

- 查看知识库详细信息
- 获取 `toc_yml` 目录结构
- 确认知识库公开性、文档数量等

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/repos/67048677"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `book_id` | 必填，知识库 ID 或 namespace |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `id` | 知识库 ID |
| `name` | 名称 |
| `namespace` | 完整路径 |
| `items_count` | 文档数量 |
| `public` | 0=私密 / 1=公开 / 2=企业内公开 |
| `toc_yml` | 目录结构（YAML 格式） |
| `creator_id` | 创建者 ID |

完整字段见 `references/api/repo_api.md`。

## 调完干嘛

- 展示知识库基本信息
- 用 `toc_yml` 分析目录结构

## 别干的事

- 别拿这个替代 `GET /api/v2/repos/:book_id/toc`（获取目录）——toc_yml 是 YAML 字符串，toc API 是结构化数组
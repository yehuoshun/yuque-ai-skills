# 获取知识库目录

## API

`GET /api/v2/repos/{book_id}/toc` — 完整文档见 `references/api/toc_api.md`。

> `book_id` 支持数字 ID 或 namespace。返回扁平数组，通过 `uuid`/`parent_uuid`/`child_uuid` 导航层级。

## 什么时候调

- 浏览知识库目录结构
- 根据目录定位文档
- 知识库问答（无索引时的替代方案）

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/repos/67048677/toc"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `book_id` | 必填，知识库 ID 或 namespace |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `uuid` | 节点唯一 ID，导航用 |
| `type` | DOC=文档 / LINK=外链 / TITLE=分组标题 |
| `title` | 节点名称 |
| `doc_id` | 文档 ID（type=DOC 时） |
| `level` | 层级（0 起） |
| `parent_uuid` | 父节点 uuid，null 为根节点 |
| `child_uuid` | 首个子节点 uuid |
| `visible` | 0=隐藏 / 1=可见 |

完整字段见 `references/api/toc_api.md`。

## 调完干嘛

- 重建目录树：按 `level` 和 `parent_uuid` 构建层级结构
- 获取一篇文档后，用目录的 `uuid` 链确定它在知识库中的位置

## 别干的事

- 别以为 `level` 是连续的——中间可能有空层
- 别直接当文档搜索用——搜索用 `GET /api/v2/search`（通用搜索）
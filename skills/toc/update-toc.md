# 更新知识库目录

## API

`PUT /api/v2/repos/{book_id}/toc` — 完整文档见 `references/api/toc_api.md` → 更新目录。

> `book_id` 支持 ID 或 namespace。一个接口覆盖创建/移动/编辑/删除四种操作，通过 `action` 区分。

## 什么时候调

- 创建文档后加入目录
- 调整目录结构（移动/排序）
- 创建分组标题或外链
- 删除目录节点

## 调用

```bash
# 创建文档节点（尾插到根目录）
curl -s -X PUT -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"action":"appendNode","action_mode":"child","type":"DOC","doc_ids":[273214401]}' \
  "https://www.yuque.com/api/v2/repos/67048677/toc"

# 创建分组标题
curl -s -X PUT -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"action":"appendNode","action_mode":"child","type":"TITLE","title":"新分组"}' \
  "https://www.yuque.com/api/v2/repos/67048677/toc"

# 删除节点
curl -s -X PUT -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"action":"removeNode","action_mode":"sibling","node_uuid":"xxx"}' \
  "https://www.yuque.com/api/v2/repos/67048677/toc"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `book_id` | 必填，知识库 ID 或 namespace |
| `action` | 必填：appendNode=尾插 / prependNode=头插 / editNode=编辑 / removeNode=删除 |
| `action_mode` | 必填：sibling=同级 / child=子级。删除时 sibling=删当前 / child=删含子节点 |
| `type` | 节点类型：DOC/LINK/TITLE（创建必填，编辑选填） |
| `doc_ids` | 文档 ID 数组 JSON（创建文档节点必填） |
| `title` | 节点名称（创建分组/外链必填） |
| `url` | 节点 URL（创建外链必填） |
| `target_uuid` | 目标节点 UUID，不填默认根节点 |
| `node_uuid` | 操作节点 UUID（移动/编辑/删除必填） |

## 返回

返回更新后的完整目录（V2TocItem 数组）。

## 调完干嘛

- 告知用户目录更新结果
- 调 `GET /api/v2/repos/:book_id/toc`（获取目录） 确认变更

## 别干的事

- 别在创建场景用 prependNode（不支持）
- 别忘记删除节点不会删关联文档——只是从目录移除
- 别传 `doc_id`（已废弃），用 `doc_ids`
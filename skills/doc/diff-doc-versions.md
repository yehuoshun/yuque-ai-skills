# yuque_diff_doc_versions

## API

无原生端点。内部调用 `GET /api/v2/doc_versions/:id` 两次拉取版本内容，本地计算 LCS diff。

## 什么时候调

- 文档被修改后想知道具体改了什么
- 版本回滚前确认差异
- 审计文档变更历史
- 对两个版本做内容对比分析

## 调用

```bash
# 方式一：通过 MCP 工具
yuque_diff_doc_versions --version_id_1 3918723977 --version_id_2 3918724518

# 方式二：直接 curl（需先获取版本内容再本地 diff）
# 步骤 1：获取版本 1
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/doc_versions/3918723977"
# 步骤 2：获取版本 2
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/doc_versions/3918724518"
# 步骤 3：对两个版本的 body 做 diff（可用 diff 命令或在线工具）
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `version_id_1` | 必填，第一个版本 ID（旧版本） |
| `version_id_2` | 必填，第二个版本 ID（新版本） |
| `max_lines` | 预览最大行数，默认 200，0=全部 |
| `context_lines` | 变更周围保留的上下文行数，默认 3，0=无上下文 |

## 返回

```json
{
  "version_1": {
    "id": 3918723977,
    "title": "文档标题",
    "created_at": "2026-06-15T04:01:10.000Z",
    "lines": 5
  },
  "version_2": {
    "id": 3918724518,
    "title": "文档标题（已修改）",
    "created_at": "2026-06-15T04:01:33.000Z",
    "lines": 7
  },
  "stats": {
    "total_lines": 8,
    "added_lines": 3,
    "removed_lines": 1,
    "unchanged_lines": 4,
    "total_changes": 4,
    "similarity_pct": 50
  },
  "diff_preview": "- 删除的行\n+ 新增的行\n  未修改的行（上下文）"
}
```

## 调完干嘛

- 解读 diff 结果给用户
- 如需回滚到旧版本，可用旧版本内容重新创建文档
- 统计信息可做变更量评估：相似度 <30% 说明大幅重写

## 别干的事

- 别对大版本做 diff（>10 万行），LCS O(m*n) 会慢
- 别传同一个 version ID，diff 没有意义
- 这是纯文本 diff，不是语义 diff——改一个标点也算 changed
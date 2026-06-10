# 获取知识库列表

## API

`GET /api/v2/users/{login}/repos` / `GET /api/v2/groups/{login}/repos` — 完整文档见 `references/api/repo_api.md`。

> `login` 支持 Login 或 ID，工具自动判断用户/团队端点（先试 users，404 回退 groups）。

## 什么时候调

- 查看用户或团队下有哪些知识库
- 需要拿到知识库 ID/slug/namespace 做后续操作
- 筛选特定类型的知识库

## 调用

```bash
# 用户知识库
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/users/yehuoshun/repos?limit=100"

# 团队知识库
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" \
  "https://www.yuque.com/api/v2/groups/uuctgr/repos?limit=100"
```

## 参数要点

| 参数 | 说明 |
|------|------|
| `login` | 必填，用户/团队的 Login 或 ID |
| `type` | Book（文档型）/ Design（画板型） |
| `offset` | 分页偏移，默认 0 |
| `limit` | 每页数量，≤100，默认 100 |
| `filterByAbility` | `create_doc` 仅返回有创建文档权限的知识库 |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `id` | 知识库 ID |
| `slug` | 路径 |
| `name` | 名称 |
| `type` | Book/Design/Sheet/Resource |
| `namespace` | 完整路径（后续所有操作需要） |
| `items_count` | 文档数量 |
| `public` | 0=私密 / 1=公开 / 2=企业内公开 |

完整字段见 `references/api/repo_api.md`。

## 调完干嘛

- 拿到 `namespace` 后去列表/创建/搜索文档
- 拿到 `id` 后去获取目录

## 别干的事

- 别拿 `namespace` 当 `login` 传——这是两个不同的东西
- 别在团队端点查用户知识库——会 404，工具已自动回退
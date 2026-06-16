# 创建知识库

## API

`POST /api/v2/users/{login}/repos` / `POST /api/v2/groups/{login}/repos` — 完整文档见 `references/api/repo_api.md` → 创建知识库。

> `login` 支持 Login 或 ID，工具自动判断用户/团队端点。

## 什么时候调

- 创建新知识库
- 批量初始化知识库

## 调用

```bash
curl -s -X POST -H "X-Auth-Token: $YUQUE_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name":"新知识库","slug":"new-book","public":0}' \
  "https://www.yuque.com/api/v2/users/yehuoshun/repos"
```

## 参数要点

| 参数 | 位置 | 类型 | 说明 |
|------|------|------|------|
| `login` | path | string | 用户/团队的 Login 或 ID（必填） |
| `name` | body | string | 知识库名称（必填） |
| `slug` | body | string | 知识库路径（可选，不传则自动生成。规则：中文→拼音首字母缩写，英文→kebab-case，加时间戳后4位） |
| `description` | body | string | 简介 |
| `public` | body | int | 0=私密 / 1=公开 / 2=企业内公开（默认 0） |
| `enhancedPrivacy` | body | bool | 增强私密性：非管理员成员也设为无权限 |

## 返回（关键字段）

| 字段 | 用途 |
|------|------|
| `id` | 新知识库 ID |
| `namespace` | 完整路径，后续操作需要 |
| `slug` | 路径 |
| `name` | 名称 |

完整字段见 `references/api/repo_api.md`。

## 调完干嘛

- 告知用户创建成功（名称 + namespace）
- 拿到 `namespace` 后即可创建文档

## 别干的事

- 别用已存在的 slug——会 422 冲突
- 别在团队端点创建用户知识库——工具已自动回退
# 语雀 AI Skills

> 纯 Skills 版 — 零 MCP 开销，`exec` + `curl` 直调语雀 API。比 MCP 版省 ~80% token，功能完全一致。

[![License](https://img.shields.io/github/license/yehuoshun/yuque-ai-skills)](./LICENSE)
[![SKILL.md](https://img.shields.io/badge/SKILL.md-执行规范-green)](./SKILL.md)

---

## vs MCP 版

| 维度 | MCP 版（yuque-ai-mcp） | Skills 版（本项目） |
|------|----------------------|-------------------|
| 安装 | clone + npm build | clone 即用 |
| 依赖 | Node.js + TypeScript | 零依赖 |
| Token 消耗 | 40 个 tool 定义常驻 context（~15K tokens） | 仅按需加载 skill 文件 |
| API 调用 | MCP tool → stdio → HTTP | `exec` + `curl` 直调 |
| 功能 | 40 个 API 端点 | 40 个 API 端点（完全相同） |
| 业务 Skills | 18 个 | 18 个（完全相同） |
| 文件上传 | ✅ | ✅（multipart/form-data curl） |
| 回收站管理 | ✅ | ✅ |
| 个人仪表盘 | ✅ | ✅ |

---

## 快速开始

### 1. 安装 Skill

```bash
cd ~/.openclaw/workspace/skills
git clone https://github.com/yehuoshun/yuque-ai-skills.git
```

### 2. 配置

```bash
cd yuque-ai-skills
cp config/yuque-config.example.json config/yuque-config.json
# 编辑填入 token、group、default_book、index_book
```

```json
{
  "token": "语雀 API Token（必填，在 https://www.yuque.com/settings/tokens 生成）",
  "group": "语雀用户名/login（必填）",
  "default_book": { "book_id": 78276514, "namespace": "yehuoshun/index-sub-1" },
  "index_book": { "book_id": 51689762, "namespace": "yehuoshun/rqgc16" },
  "user_id": "用户 ID（文件上传必填）",
  "cookie": "浏览器 Cookie（回收站/仪表盘/文件上传必填）",
  "ctoken": "CSRF Token（从 Cookie 提取 yuque_ctoken 值）"
}
```

### 3. 使用

直接在对话中说「语雀」相关内容即可触发：

- 「搜一下语雀里关于 Kubernetes 的文档」
- 「把知识库 A 里去年之前的文档归档」
- 「帮我看看回收站有什么」
- 「分析下我的写作统计」
- 「批量翻译这篇文档」

---

## 架构

```
SKILL.md         ← 端点速查表 + 业务路由
    ↓
业务 Skills      ← 18 个 Markdown（batch/write/map）
    ↓
LLM Agent        ← exec + curl → 语雀 API
```

---

## API 覆盖（40+ 端点）

### v2 OpenAPI（Token 认证）

- 用户/搜索/知识库 CRUD/文档 CRUD/版本历史/目录管理
- 小记 CRUD + 软删除/恢复
- 群组成员管理
- 四维统计（团队/成员/知识库/文档）

### Web API（Cookie 认证）

- 回收站（列表/恢复/彻底删除）
- 个人写作仪表盘（知识库/文档/编辑/字数/社交/小记全维度）
- 文件上传 CDN

---

## 业务 Skills（18 个）

| Skill | 说明 |
|-------|------|
| [archive](skills/batch/archive.md) | 批量归档/备份旧文档 |
| [audit](skills/batch/audit.md) | 版本审计 & 变更追踪 |
| [classify](skills/batch/classify.md) | 智能分类打标 |
| [dashboard](skills/batch/dashboard.md) | 运营仪表盘 |
| [format](skills/batch/format.md) | 批量格式标准化 |
| [import](skills/batch/import.md) | 外部文档导入 |
| [merge](skills/batch/merge.md) | 文档合并 |
| [rebuild-toc](skills/batch/rebuild-toc.md) | 目录智能重构 |
| [rename](skills/batch/rename.md) | 批量重命名 |
| [split](skills/batch/split.md) | 长文拆分 |
| [summarize](skills/batch/summarize.md) | 多粒度智能摘要 |
| [sync](skills/batch/sync.md) | 文档镜像同步 |
| [translate](skills/batch/translate.md) | AI 批量翻译 |
| [polish](skills/write/polish.md) | AI 写作工作室 |
| [digest](skills/map/digest.md) | 阅读摘录 |
| [inbox](skills/map/inbox.md) | 碎片收集整理 |
| [knowledge](skills/map/knowledge.md) | 文档关联图谱 |
| [search](skills/map/search.md) | 知识库搜索 & 索引 |

---

## 免责声明

- 本项目**非语雀官方产品**
- 使用前请**备份重要数据**，误操作可能导致数据丢失
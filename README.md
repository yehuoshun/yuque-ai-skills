# 语雀 AI Skill

[![License](https://img.shields.io/github/license/yehuoshun/yuque-ai-mcp)](./LICENSE)

---

## ⚠️ 免责声明

- 本项目**非语雀官方产品**，与蚂蚁集团/语雀团队无任何关联
- 使用前请**备份重要数据**。本工具涉及文档创建/修改/删除，误操作可能导致数据丢失
- 作者对因使用本工具造成的任何数据丢失、账号限制或其他后果**不承担责任**
- 请遵守[语雀服务协议](https://www.yuque.com/yuque/qeyyk7/bl95f1imynp9u6pg?view=doc_embed&title=0)，控制请求频率避免触发限流

---

## 架构

```
yuque-mcp (MCP Server)     ← 47 个 MCP Tools（CRUD/搜索/导入/统计/群组/回收站/目录增强）
    ↓
业务 Skills                ← 24 个 Skill Markdown（manage/transform/insight/collect/write 五分类）
    ↓
LLM Agent                  ← 问答编排 & 业务流转
```

> 📦 MCP Server 源码在 [yuque-ai-mcp](https://github.com/yehuoshun/yuque-ai-mcp) 仓库。本仓库只包含 Skill 层（SKILL.md + skills/ + references/）。

---

## 知识库问答

**TOC 树导航（零索引）**：无需预处理、无需索引维护、无需向量库。

```
yuque_list_repos → LLM 选知识库
       ↓
yuque_list_toc  → LLM 导航选文档
       ↓
yuque_get_doc   → LLM 生成答案
```

标题质量好的知识库即开即用。标题泛化不够时，LLM 可先用 `yuque_search` 在全库范围搜索，再用 `yuque_get_doc` 读具体内容。

---

## 业务 Skills

基于 MCP 47 tools 的高层业务能力。全部遵循：先预览后确认、单篇隔离不传染、上限 100 篇、结束出报告。

### 知识管理（manage/）

| Skill | 说明 |
|-------|------|
| [archive](skills/manage/archive.md) | 批量归档/备份旧文档（归档移动 / 备份复制两种模式） |
| [backup](skills/manage/backup.md) | 知识库完整下载到本地（全量导出，含TOC结构+图片+附件） |
| [purify](skills/manage/purify.md) | 知识库净化（清洗标题/内容/结构 → 去广告去HTML垃圾 → 重建新库） |
| [classify](skills/manage/classify.md) | 智能分类打标（AI 分析主题 → 自动设计目录树 → 重建结构） |
| [format](skills/manage/format.md) | 批量格式标准化（预设风格 / 参考文档 / 自定义规则三种来源） |
| [import](skills/manage/import.md) | 外部文档导入（本地/Obsidian/Notion，格式适配 + 图片上传） |
| [merge](skills/manage/merge.md) | 多篇文档合并为单篇长文 |
| [rebuild-toc](skills/manage/rebuild-toc.md) | 目录智能重构（保留意图优化现有结构） |
| [rename](skills/manage/rename.md) | 批量重命名（前缀/后缀/序号/查找替换/正则/模板/移除） |
| [sync](skills/manage/sync.md) | 文档镜像 & 知识库同步（单向/双向/增量/差异检测） |

### 内容加工（transform/）

| Skill | 说明 |
|-------|------|
| [split](skills/transform/split.md) | 长文按标题层级自动拆分为多篇 |
| [summarize](skills/transform/summarize.md) | 多粒度智能摘要（L1-L4 四级 + 单篇/多篇/知识库） |
| [translate](skills/transform/translate.md) | AI 批量翻译（单篇/批量/多语言/增量，保留 Markdown 格式） |

### 洞察分析（insight/）

| Skill | 说明 |
|-------|------|
| [audit](skills/insight/audit.md) | 文档版本审计 & 变更追踪（变更日报/单篇历史/版本对比/协作追踪） |
| [dashboard](skills/insight/dashboard.md) | 知识库运营仪表盘（周报/概览/成员详情，纯只读） |
| [digest](skills/insight/digest.md) | 阅读摘录（核心观点/金句/行动项/疑思五维 + 知识卡片） |
| [knowledge](skills/insight/knowledge.md) | 知识图谱（共现图/Louvain社区检测/索引质量检查/章节树） |
| [search](skills/insight/search.md) | 知识库搜索（N 路并行 + 去重 + 降级） |

### 碎片收集（collect/）

| Skill | 说明 |
|-------|------|
| [inbox](skills/collect/inbox.md) | 碎片收集整理（三种模式 + 可配置清理策略） |

### 写作辅助（write/）

| Skill | 说明 |
|-------|------|
| [polish](skills/write/polish.md) | AI 写作工作室（触发词路由 → 风格分析/打磨/迁移/模板） |
| [analyze](skills/write/analyze.md) | 写作风格分析（10 维画像 + 单篇/对比/知识库模式） |
| [refine](skills/write/refine.md) | 笔记打磨（三级打磨 + N 次打磨 + 批量模式） |
| [transfer](skills/write/transfer.md) | 风格迁移（提取风格画像 → 重写 → 对照） |
| [template](skills/write/template.md) | 模板写作（6 种预设模板：技术方案/周报/复盘/纪要/API/PRD） |

---

## 项目结构

```
yuque-ai-skills/
├── SKILL.md                  # AI Agent 执行规范
├── README.md                 # 本文件
├── references/
│   └── api_reference.md      # 语雀 OpenAPI 完整参考
├── skills/                   # 业务 Skills（24 个）
│   ├── manage/               # 知识管理（10 个）
│   ├── transform/            # 内容加工（3 个）
│   ├── insight/              # 洞察分析（5 个）
│   ├── collect/              # 碎片收集（1 个）
│   └── write/                # 写作辅助（5 个）
└── config/
    └── yuque-config.example.json
```

---

## API 参考

基地址：`https://www.yuque.com/api/v2`

完整端点/参数/错误码/限流/回收站 Web API → **[references/api_reference.md](./references/api_reference.md)**

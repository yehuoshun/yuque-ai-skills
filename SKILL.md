# yuque-ai-skills

> 语雀 Skill 层 — 工具使用指导、场景模式、最佳实践。不包含可执行代码，只做用法文档。

## 触发

当用户提到以下关键词时，加载 MCP 工具集并结合本 Skill 指导执行：

- **操作类**：语雀、yuque、知识库、文档、团队、回收站、导出、导入、小记
- **管理类**：整理语雀、归档、备份、格式化、重命名、合并、分类
- **搜索类**：在语雀搜索、找文档

## 工具域

| 域 | 目录 | 说明 |
|----|------|------|
| user | `skills/user/` | 用户信息、心跳、团队 |
| group | `skills/group/` | 团队成员管理 |
| repo | `skills/repo/` | 知识库 CRUD |
| doc | `skills/doc/` | 文档读写 |
| search | `skills/search/` | 搜索 |
| kb | `skills/kb/` | 知识库问答（TOC 导航） |
| recycle | `skills/recycle/` | 回收站 |
| export | `skills/export/` | 导出 |
| import | `skills/import/` | 导入 |
| note | `skills/note/` | 小记 |

## API 使用指南

调用任何 API 前，**必须先读对应的 skill 文件**。完整字段见 `references/api/` 下的对应 API 文档。

**错误码通用**：所有 API 共用错误码，见 `references/api/errors.md`。
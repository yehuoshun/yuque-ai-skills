# search — 搜索（3 工具）

> 通用约定见 `skills/common/conventions.md`，错误码见 `skills/common/errors.md`

## 端点速查

| 工具 | 端点 | 说明 |
|------|------|------|
| `yuque_search` | `GET /search` (API v2) | 通用全文搜索文档/知识库 |
| `yuque_rag_search` | HyDE + search + get_doc | RAG 检索增强搜索，自动获取文档内容 |
| `yuque_web_search` | `GET /api/zsearch` (Cookie) | Cookie 态搜索，返回完整文档对象 + 精确总数 + 高亮摘要 |

## 工具对比

| | `yuque_search` | `yuque_rag_search` | `yuque_web_search` |
|---|---|---|---|
| 认证 | Token | Token | Cookie + ctoken |
| 返回 | 摘要 | 答案 + 来源 | 完整文档对象 |
| 总数 | ❌ | ❌ | ✅ `totalHits` |
| 高亮 | ❌ | ❌ | ✅ `abstract` |
| 搜用户 | ❌ | ❌ | ✅ |
| 自动读内容 | ❌ | ✅ | ✅ |

## 详细文档

| 工具 | Skill 文件 |
|------|-----------|
| `yuque_search` | `skills/search/search.md` |
| `yuque_rag_search` | `skills/search/rag-search.md` |
| `yuque_web_search` | `skills/search/web-search.md` |

## API 参考

完整 API 字段定义见 `references/api/search_api.md`

# HyDE 降级搜索 (yuque_hyde_search)

当索引搜索（Page Index / QMD）无结果时，通过 HyDE（Hypothetical Document Embeddings）策略降级到语雀全库搜索。

## 流程

```
用户提问
  ↓
LLM 生成假设文档摘要 + 搜索关键词
  ↓
关键词过滤（去除语雀分词器不支持的 - . _ 符号）
  ↓
并发多路搜索（每个关键词一个请求）
  ↓
按 doc_id 去重合并
  ↓
返回结果
```

## 关键词过滤规则

语雀分词器会对 `-` `.` `_` 进行切分，导致含这些符号的技术术语搜不到。

| 原始词 | 过滤后 | 原因 |
|--------|--------|------|
| `z-fighting` | `z fighting` + `zfighting` | `-` 被切分，生成空格版 + 拼接版 |
| `gl.enable` | `gl enable` + `glenable` | `.` 被切分 |
| `depth_test` | `depth test` + `depthtest` | `_` 被切分 |
| `深度测试` | `深度测试` | 中文不受影响 |
| `WebGL` | `WebGL` | 英文单词不受影响 |
| `[]()` | 丢弃 | 纯符号被忽略 |

## LLM 配置

需要设置以下环境变量：

| 变量 | 说明 | 默认值 |
|------|------|--------|
| `HYDE_LLM_ENDPOINT` | LLM API 地址 | `$OPENAI_BASE_URL` |
| `HYDE_LLM_KEY` | LLM API Key | `$OPENAI_API_KEY` |
| `HYDE_LLM_MODEL` | 模型名 | `gpt-4o-mini` |

未配置时自动降级为基础关键词提取（从提问中分词）。

## 调用示例

```json
{
  "q": "WebGL 怎么处理 z-fighting 问题",
  "scope": "yehuoshun",
  "max_results": 20
}
```

## 返回结构

```json
{
  "meta": {
    "total": 15,
    "hyde_doc": "假设文档摘要...",
    "keywords_used": ["z fighting", "zfighting", "深度冲突", "WebGL"],
    "raw_keywords": ["z-fighting", "深度冲突", "WebGL"]
  },
  "data": [
    {
      "id": 12345,
      "doc_id": 12345,
      "title": "WebGL 深度测试详解",
      "summary": "讲解深度测试原理...",
      "url": "https://www.yuque.com/...",
      "book_name": "图形学笔记",
      "book_slug": "graphics",
      "updated_at": "2026-06-09T12:00:00Z"
    }
  ]
}
```
# HyDE 降级搜索

当索引搜索（Page Index / QMD）无结果时，使用 HyDE 策略降级到语雀全库搜索。

## 流程

```
用户提问
  ↓
Agent 用 LLM 生成假设文档摘要 + 搜索关键词（HyDE）
  ↓
调 yuque_hyde_search(keywords="关键词1,关键词2,...")
  ↓
MCP 自动过滤语雀分词器不支持的符号（- . _）
  ↓
并发多路搜索 → doc_id 去重合并
  ↓
返回结果
```

## Agent 侧 HyDE Prompt

生成关键词时使用以下 prompt：

```
你是一个知识库搜索助手。根据用户提问，生成 5-10 个搜索关键词用于语雀全文搜索。

## 关键词要求
- **中文优先**，其次是英文单词
- **不要包含连字符(-)、点号(.)、下划线(_)**——展开成中文或空格分隔
  - 错误：z-fighting, gl.enable, depth_test
  - 正确：深度冲突 zfighting, gl开启深度测试, depth test
- 覆盖同义词、缩写展开、中英文对照
- 技术术语优先用中文表达，英文作为补充

## 输出格式
只输出逗号分隔的关键词，不要其他内容：
关键词1,关键词2,关键词3,...

## 用户提问
{用户提问}
```

## 调用示例

```
yuque_hyde_search(keywords="深度冲突,zfighting,深度测试,WebGL,深度缓冲,depth test", scope="yehuoshun")
```

## 返回结构

```json
{
  "meta": {
    "total": 15,
    "method": "keyword_search",
    "keywords_used": ["深度冲突", "zfighting", "深度测试", "WebGL", "深度缓冲", "depth test"],
    "raw_keywords": ["深度冲突", "zfighting", "深度测试", "WebGL", "深度缓冲", "depth test"]
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

## 降级链

```
QMD 索引搜索 → 命中？→ 读原文回答
  ↓ 未命中
yuque_hyde_search（HyDE 关键词搜索）→ 命中？→ 读原文回答
  ↓ 未命中
yuque_search（原始提问直接搜，最后兜底）
```
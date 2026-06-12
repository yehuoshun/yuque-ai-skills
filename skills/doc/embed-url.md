# yuque_embed_url

> 端点：无（纯工具函数，基于语雀嵌入文档阅读器规范）
> 说明：根据文档 URL 或 doc_id + book 信息，生成带 `view=doc_embed` 参数的嵌入阅读器 URL 和 iframe 代码
> 参考：[语雀嵌入文档阅读页 API 文档](https://www.yuque.com/yuque/developer/embed)

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `url` | string | 二选一 | 语雀文档完整 URL，如 `https://www.yuque.com/yuque/developer/embed` |
| `doc_id` | string | 二选一 | 文档 ID，需配合 `book_id` |
| `book_id` | string | 配合 doc_id | 知识库 ID 或 namespace（如 `yuque/developer`） |
| `from` | string | **必填** | 调用方名称（appname），建议填团队/应用的英文名 |
| `title` | number | 可选 | 是否显示标题：`1`=显示，`0`=隐藏 |
| `outline` | number | 可选 | 是否显示右侧大纲：`1`=显示，`0`=隐藏 |
| `translate` | string | 可选 | 翻译语种代码（见下方支持列表） |

### 支持的翻译语种

en / zh / ru / pt / es / fr / ja / ar / de / it / ko / tr / vi / pl / he / id / hi / nl / th

## 响应

```json
{
  "embed_url": "https://www.yuque.com/yuque/blog/digital_garden?view=doc_embed&from=myapp",
  "iframe_code": "<iframe src=\"https://www.yuque.com/yuque/blog/digital_garden?view=doc_embed&from=myapp\" width=\"100%\" height=\"600\" frameborder=\"0\"></iframe>",
  "params": {
    "view": "doc_embed",
    "from": "myapp",
    "title": "默认（显示）",
    "outline": "默认（显示）",
    "translate": "无"
  },
  "note": "嵌入模式仅支持公开文档。私密文档请通过 API 获取内容后自行渲染。",
  "supported_languages": { "en": "英语", "zh": "中文", ... }
}
```

## 使用场景

- 需要在第三方系统中嵌入展示语雀文档（如帮助中心、知识库门户）
- 生成免登录的文档阅读页链接
- 配合 `title=0&outline=0` 实现最简嵌入式阅读器
- 多语言场景通过 `translate` 参数自动翻译文档

## 注意事项

1. **仅支持公开文档**：嵌入模式受浏览器安全策略限制，私密文档无法通过 iframe 嵌入。如需集成私密文档，应通过 API 获取内容后自行渲染
2. **无需申请**：`from` 参数无需申请，直接填写团队/应用英文名即可
3. **标题和大纲**：默认显示，可通过参数关闭
4. **翻译功能**：基于语雀内置翻译能力，非实时翻译，可能需要等待处理
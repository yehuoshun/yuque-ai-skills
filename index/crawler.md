# crawler — 网页爬虫（4 工具）

> 通用约定见 `common/conventions.md`，错误码见 `common/errors.md`

## 端点速查

| 工具 | 端点 | 说明 |
|------|------|------|
| `yuque_crawl_fetch` | HTTP GET | 抓取网页原始 HTML |
| `yuque_crawl_extract` | CSS 选择器（纯正则） | 从 HTML 提取内容/属性 |
| `yuque_crawl_save` | 去重 + 写入语雀 | 接收 Agent 清洗后的内容，去重写入 |
| `yuque_crawl_schedule` | 分析 + 写入配置 | 分析抓取频率，生成推荐时间 |

> **清洗规范**：Agent 负责 fetch → 提取正文 → HTML→Markdown → 传干净 body 给 `crawl_save`。详见 `references/api/crawler_api.md`。

## 详细文档

| 工具 | Skill 文件 |
|------|-----------|
| `yuque_crawl_fetch` | `skills/crawler/yuque_crawl_fetch.md` |
| `yuque_crawl_extract` | `skills/crawler/yuque_crawl_extract.md` |
| `yuque_crawl_save` | `skills/crawler/yuque_crawl_save.md` |
| `yuque_crawl_schedule` | `skills/crawler/crawl-schedule.md` |

## API 参考

完整 API 字段定义见 `references/api/crawler_api.md`

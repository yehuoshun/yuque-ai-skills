# rss — RSS 抓取（3 工具）

> 通用约定见 `skills/common/conventions.md`，错误码见 `skills/common/errors.md`

## 端点速查

| 工具 | 端点 | 说明 |
|------|------|------|
| `yuque_rss_list_sources` | 本地配置读取 | 列出所有可用 RSS 数据源及 feed 类型 |
| `yuque_rss_fetch` | HTTP GET Feed + 解析 + 去重 + 写入 | 抓取 RSS/Atom Feed，去重后写入语雀 |
| `yuque_rss_schedule` | 分析 + 写入配置 | 分析更新频率，生成推荐抓取时间 |

## 详细文档

| 工具 | Skill 文件 |
|------|-----------|
| `yuque_rss_list_sources` | `skills/rss/rss-list-sources.md` |
| `yuque_rss_fetch` | `skills/rss/rss-fetch.md` |
| `yuque_rss_schedule` | `skills/rss/rss-schedule.md` |

## API 参考

完整 API 字段定义见 `references/api/rss_api.md`

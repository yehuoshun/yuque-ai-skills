# yuque_rss_schedule

> 端点：无（RSS 定时抓取策略分析）
> 说明：根据 KV 去重数据中的历史文章时间戳，分析各作者更新频率，输出分档抓取策略建议

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `source` | string | ✅ | 数据源 key，如 `cnblogs` |
| `kv_namespace` | string | ❌ | KV namespace，默认等于 source |
| `raw` | boolean | ❌ | 返回原始 JSON（默认 false） |

## 响应

```json
{
  "namespace": "cnblogs",
  "source": "cnblogs",
  "sourceName": "博客园",
  "totalAuthors": 42,
  "totalArticles": 156,
  "bands": [
    {
      "label": "高频",
      "range": "≤2 天",
      "interval": "每 6h",
      "intervalHours": 6,
      "authors": ["张三", "李四"]
    },
    {
      "label": "中频",
      "range": "3-7 天",
      "interval": "每 12h",
      "intervalHours": 12,
      "authors": ["王五", "赵六"]
    }
  ],
  "recommendations": [
    {
      "feed": "sitehome",
      "label": "首页最新",
      "frequency": "每 6h",
      "reason": "首页/推荐内容更新频繁，建议高频抓取"
    }
  ],
  "authors": [
    {
      "name": "张三",
      "articleCount": 12,
      "firstSeen": "2026-01-15",
      "lastSeen": "2026-06-18",
      "avgIntervalDays": 1.5,
      "band": "高频"
    }
  ]
}
```

## 频率分档规则

| 分档 | 条件 | 建议间隔 |
|------|------|----------|
| 高频 | 平均 ≤2 天一更 | 每 6h |
| 中频 | 平均 3-7 天一更 | 每 12h |
| 低频 | 平均 7-30 天一更 | 每天 1 次 |
| 休眠 | >30 天未更新 | 每 3 天 1 次 |

## 使用场景

- 分析 RSS 源的作者更新频率，规划 cron 定时抓取间隔
- 在设置定时抓取任务前，先用此工具了解最佳抓取频率
- 定期重新分析，动态调整抓取策略

## 前置条件

- KV 功能已启用（`config.json` 中 `kv.enabled: true`）
- KV namespace 中有已抓取的文章记录（先跑过 `yuque_rss_fetch`）
- KV 记录格式需包含时间戳和作者信息（v2.6.0+ 的 fetch 自动生成）

## 注意事项

- 数据量越大分析越准确，建议积累至少 10 篇以上再分析
- 旧格式 KV 数据（v2.5.0 及之前）不含时间戳，会提示重新抓取
- 分析结果仅供参考，实际抓取频率可根据需求调整

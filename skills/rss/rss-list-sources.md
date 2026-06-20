# yuque_rss_list_sources

> 端点：无（RSS 数据源列表）
> 说明：列出所有可用 RSS 数据源及 feed 类型

## 参数

无参数。

## 响应

```json
{
  "sources": [
    {
      "key": "cnblogs",
      "name": "博客园",
      "description": "开发者的网上家园，技术博客聚合平台",
      "feeds": [
        {
          "key": "sitehome",
          "label": "首页最新",
          "description": "博客园首页最新发布的文章",
          "has_template": false,
          "params": []
        },
        {
          "key": "user",
          "label": "用户博客",
          "description": "指定用户的博客文章",
          "has_template": true,
          "params": [
            { "name": "username", "type": "string", "description": "博客园用户名", "required": true }
          ]
        }
      ]
    }
  ],
  "total": 1
}
```

## 使用场景

- 在调用 `yuque_rss_fetch` 之前，先查有哪些数据源和 feed 类型可用
- 查看某个 feed 是否需要额外参数（`has_template` 为 true 时需要传 `feed_params`）

## 注意事项

- 数据源配置在 `config.json` 的 `rss.sources` 中，新增数据源只需改配置，无需改代码
- 此 tool 只读配置，不发起网络请求
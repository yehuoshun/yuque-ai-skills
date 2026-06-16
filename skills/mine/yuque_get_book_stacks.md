# yuque_get_book_stacks

获取当前用户的知识库分组（书架）列表。

## 端点

```
GET /api/mine/book_stacks
```

## 认证

需要 Cookie 登录态（`cookie` + `ctoken`），在 `config/config.json` 中配置。

## 参数

无参数。

## 返回

```json
{
  "stacks": [
    {
      "id": 26776447,
      "name": "Java",
      "rank": 0,
      "books": [
        {
          "id": 24255234,
          "type": "Book",
          "slug": "itsn3r",
          "name": "Java",
          "description": "黑马程序员...",
          "items_count": 100
        }
      ]
    }
  ]
}
```

## 使用场景

- 查看所有知识库分组结构
- 快速定位知识库 ID
- 配合其他工具使用（如 `yuque_get_repo`、`yuque_list_docs`）

## 注意事项

- 此接口走 Web API（非 v2 OpenAPI），需要浏览器 Cookie 认证
- Cookie 过期后需重新获取

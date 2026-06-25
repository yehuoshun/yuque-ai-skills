# 认证

## Token 认证（API v2）

所有 API v2 端点使用 `X-Auth-Token` 头：

```bash
curl -H "X-Auth-Token: $YUQUE_TOKEN" "https://www.yuque.com/api/v2/..."
```

- Token 在语雀个人设置 → Token 管理 中生成
- 权限范围：Token 生成时选择，需包含对应操作的权限

## Cookie 认证（Web API）

部分功能需要 Cookie + CSRF Token：

- **需要 Cookie 的功能**：回收站操作、上传附件、mine（书架/编辑中心）、web-search
- **获取方式**：浏览器登录语雀后从 DevTools → Application → Cookies 复制
- **ctoken**：Cookie 中的 `ctoken` 字段值，用于 `x-csrf-token` 请求头

```bash
curl 'https://www.yuque.com/api/zsearch?...' \
  -H 'x-csrf-token: {ctoken}' \
  -b '{cookie}'
```

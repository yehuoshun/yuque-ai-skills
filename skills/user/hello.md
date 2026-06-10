# 心跳检测

## API

`GET /api/v2/hello` — 完整文档见 `references/api/user_api.md` → 心跳检测。

## 什么时候调

- 启动时验证 Token 是否有效
- 排查 401 错误时确认连通性
- 用户问「Token 还能用吗」「连得上语雀吗」

## 调用

```bash
curl -s -H "X-Auth-Token: $YUQUE_TOKEN" "https://www.yuque.com/api/v2/hello"
```

## 返回

```json
{
  "message": "Hello 夜灬瞬"
}
```

## 调完干嘛

- 成功 → Token 有效，继续操作
- 失败 → 检查 Token 是否过期，提示用户更新

## 别干的事

- 别每次操作前都调——只在启动或排障时用
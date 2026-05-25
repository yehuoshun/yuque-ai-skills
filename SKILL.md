# 语雀 AI Skills

> 纯 Skill 版 — 零 MCP 开销，curl 直调语雀 API。Agent 用 `exec` 执行 curl 命令完成所有操作。

## 核心原则

**所有语雀 API 调用通过 `exec` + `curl` 完成，不走 MCP。** 每次调用前从 `config/yuque-config.json` 读取 Token 和配置。

```bash
TOKEN=$(cat config/yuque-config.json | python3 -c "import sys,json; print(json.load(sys.stdin)['token'])")
# GET 示例
curl -s "https://www.yuque.com/api/v2/repos/78276514/docs" -H "X-Auth-Token: $TOKEN"
```

## 配置

从 `config/yuque-config.json` 读取：

```json
{
  "token": "语雀 API Token",
  "group": "用户名/login",
  "default_book": { "book_id": 78276514, "namespace": "namespace" },
  "index_book": { "book_id": 51689762, "namespace": "namespace" },
  "cookie": "Cookie 字符串（文件上传/回收站/仪表盘必填）",
  "ctoken": "CSRF Token（从 Cookie 提取 yuque_ctoken 的值）",
  "user_id": "用户 ID"
}
```

读取配置的标准方式：
```bash
eval $(python3 -c "
import json
c = json.load(open('config/yuque-config.json'))
print(f'YUQUE_TOKEN={c[\"token\"]}')
print(f'YUQUE_GROUP={c[\"group\"]}')
print(f'DEFAULT_BOOK_ID={c[\"default_book\"][\"book_id\"]}')
print(f'DEFAULT_BOOK_NS={c[\"default_book\"][\"namespace\"]}')
print(f'INDEX_BOOK_ID={c[\"index_book\"][\"book_id\"]}')
print(f'INDEX_BOOK_NS={c[\"index_book\"][\"namespace\"]}')
print(f'YUQUE_COOKIE=\"{c.get(\"cookie\",\"\")}\"')
print(f'YUQUE_CTOKEN={c.get(\"ctoken\",\"\")}')
print(f'YUQUE_USER_ID={c.get(\"user_id\",\"\")}')
")
```

## API 端点速查

基地址：`https://www.yuque.com/api/v2`

### v2 API（Token 认证）

| 操作 | 方法 | 端点 | 说明 |
|------|------|------|------|
| 用户信息 | GET | `/user` | 当前用户 |
| 连通测试 | GET | `/hello` | Token 验证 |
| 搜索 | GET | `/search?q={q}&type=doc&scope={scope}&page={page}` | 全库搜索 |
| 列出知识库 | GET | `/users/{login}/repos` | |
| 获取知识库 | GET | `/repos/{id_or_ns}` | |
| 创建知识库 | POST | `/users/{login}/repos` | body: `{name, slug, description, public}` |
| 更新知识库 | PUT | `/repos/{id_or_ns}` | body: `{name, slug, description, public, toc}` |
| 删除知识库 | DELETE | `/repos/{id_or_ns}` | ⚠️ 硬删除 |
| 列出文档 | GET | `/repos/{book_id}/docs?offset=0&limit=100` | |
| 获取文档 | GET | `/repos/{book_id}/docs/{doc_id}` | `?raw=1` 返回纯文本 |
| 创建文档 | POST | `/repos/{book_id}/docs` | body: `{title, body, format}` |
| 更新文档 | PUT | `/repos/{book_id}/docs/{doc_id}` | body: `{title, body, slug, format, public}` |
| 删除文档 | DELETE | `/repos/{book_id}/docs/{doc_id}` | ⚠️ 硬删除 |
| 版本历史 | GET | `/doc_versions?doc_id={doc_id}` | |
| 版本详情 | GET | `/doc_versions/{version_id}` | |
| 获取目录 | GET | `/repos/{book_id}/toc` | |
| 更新目录 | PUT | `/repos/{book_id}/toc` | body: `{action, action_mode, type, doc_ids, target_uuid}` |
| 列出小记 | GET | `/notes?page=1&limit=20&status=0` | status=9 查已删除 |
| 获取小记 | GET | `/notes/{note_id}` | content 是嵌套对象 |
| 创建小记 | POST | `/notes` | body: `{body}` |
| 更新小记 | PUT | `/notes/{note_id}` | body: `{source, html, abstract, status}` |
| 软删小记 | PUT | `/notes/{note_id}` | body 同上 + `status: 9` |
| 恢复小记 | PUT | `/notes/{note_id}` | body 同上 + `status: 0` |
| 群组成员 | GET | `/groups/{login}/users?page=1&limit=100` | |
| 变更角色 | PUT | `/groups/{login}/users/{id}` | body: `{role: 0\|1\|2}` |
| 移除成员 | DELETE | `/groups/{login}/users/{id}` | ⚠️ 硬删除 |
| 团队统计 | GET | `/groups/{login}/statistics` | |
| 成员统计 | GET | `/groups/{login}/statistics/members` | |
| 知识库统计 | GET | `/groups/{login}/statistics/books` | |
| 文档统计 | GET | `/groups/{login}/statistics/docs` | |

### Web API（Cookie 认证）

Web API 的通用 curl 模板：

```bash
curl -s "URL" \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -H "x-csrf-token: $YUQUE_CTOKEN" \
  -b "$YUQUE_COOKIE" \
  -H "Referer: REFERER_URL"
```

| 操作 | 方法 | 端点 | Referer |
|------|------|------|---------|
| 回收站列表 | GET | `https://www.yuque.com/api/mine/recycles?offset=0&limit=50` | `dashboard/recycles` |
| 恢复回收站 | PUT | `https://www.yuque.com/api/mine/recycles/{id}/restore` | `dashboard/recycles` |
| 彻底删除 | DELETE | `https://www.yuque.com/api/mine/recycles/{id}` | `dashboard/recycles` |
| 个人仪表盘 | GET | `https://www.yuque.com/api/mine/editor_center` | `settings/stats` |
| 上传文件 | POST | `https://www.yuque.com/api/upload/attach?type=attachment` | `yuque.com/` |

## 业务 Skills

| Skill | 说明 |
|-------|------|
| [archive](skills/batch/archive.md) | 批量归档/备份 |
| [audit](skills/batch/audit.md) | 版本审计 & 变更追踪 |
| [classify](skills/batch/classify.md) | 智能分类打标 |
| [dashboard](skills/batch/dashboard.md) | 运营仪表盘 |
| [format](skills/batch/format.md) | 批量格式标准化 |
| [import](skills/batch/import.md) | 外部文档导入 |
| [merge](skills/batch/merge.md) | 文档合并 |
| [rebuild-toc](skills/batch/rebuild-toc.md) | 目录智能重构 |
| [rename](skills/batch/rename.md) | 批量重命名 |
| [split](skills/batch/split.md) | 长文拆分 |
| [summarize](skills/batch/summarize.md) | 多粒度智能摘要 |
| [sync](skills/batch/sync.md) | 文档镜像同步 |
| [translate](skills/batch/translate.md) | AI 批量翻译 |
| [polish](skills/write/polish.md) | AI 写作工作室 |
| [digest](skills/map/digest.md) | 阅读摘录 |
| [inbox](skills/map/inbox.md) | 碎片收集整理 |
| [knowledge](skills/map/knowledge.md) | 文档关联图谱 |
| [search](skills/map/search.md) | 知识库搜索 & 索引 |

## 执行规则

1. **读配置**：每次操作前从 config 读 Token/cookie
2. **先预览**：批量操作先列清单，确认再执行
3. **单篇隔离**：一篇失败不传染其他
4. **上限 100**：批量操作单次不超过 100 篇
5. **结束出报告**：完成后总结成功/失败/跳过

## API 详细参考

完整参数/错误码/限流 → **[references/api_reference.md](references/api_reference.md)**
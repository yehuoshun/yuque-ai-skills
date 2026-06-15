# yuque_export_repo

> 端点：批量操作（无对应单一 API 端点）
> 说明：批量导出知识库全部文档为 Markdown 文件，按 TOC 目录结构组织，文件以标题命名，自动生成 INDEX.md 和 GRAPH.md

## 触发场景

- 用户说「导出语雀知识库」「备份知识库」「下载全部文档」
- 需要将语雀文档迁移到本地或其他平台
- 批量获取文档内容做离线分析

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `book_id` | string | ✅ | 知识库 ID 或 namespace（如 `group/book_slug`） |
| `output_dir` | string | ❌ | 输出目录路径，默认 `./yuque-export/<book_slug>/` |
| `download_images` | boolean | ❌ | 是否下载图片到本地，默认 true |
| `raw_body` | boolean | ❌ | 是否直接输出原始 body（不转 Markdown），默认 false |

## 响应

```json
{
  "status": "done",
  "repo": { "id": "xxx", "slug": "xxx", "name": "xxx" },
  "output_dir": "/tmp/yuque-export/xxx",
  "summary": {
    "total_docs": 10,
    "ok": 9,
    "error": 1,
    "images_downloaded": 15,
    "images_failed": 2
  },
  "index_file": "/tmp/yuque-export/xxx/INDEX.md",
  "graph_file": "/tmp/yuque-export/xxx/GRAPH.md",
  "files": [
    {
      "doc_id": 123,
      "slug": "doc-slug",
      "title": "文档标题",
      "status": "ok",
      "file": "/tmp/yuque-export/xxx/目录名/文档标题.md",
      "dir": "目录名",
      "images_downloaded": 3,
      "images_failed": 1
    }
  ]
}
```

## 工作原理

1. 获取 TOC 目录树 → 构建目录结构映射
2. 分页获取知识库全部文档列表（每页 100 篇）
3. 逐个获取文档详情（包含 body + body_html）
4. 从 body_html 中提取图片/附件 URL
5. 尝试下载图片到 `output_dir/images/`，下载附件到 `output_dir/attachments/`
6. **下载失败降级**：保留原始 CDN 链接，不丢数据
7. 按 TOC 目录结构创建文件夹，文件以**文档标题**命名（含 YAML frontmatter）
8. 生成 `INDEX.md`（目录树 + 文档列表）和 `GRAPH.md`（文档间引用关系）
9. 返回完整导出报告

## 目录结构示例

```
yuque-export/<book_slug>/
├── INDEX.md              # 目录索引（TOC 树 + 文档列表）
├── GRAPH.md              # 文档引用关系图
├── images/               # 图片资源
├── attachments/          # 附件资源
├── 入门指南/
│   ├── 快速开始.md
│   └── 安装说明.md
├── API 文档/
│   ├── 认证方式.md
│   └── 接口列表.md
└── 常见问题.md           # 根级文档
```

## 文档格式处理

| 源格式 | 导出行为 |
|--------|---------|
| `markdown` | 直接使用 body 字段 |
| `html` | body_html → 简单 Markdown 转换 |
| `lake` | body_html → 简单 Markdown 转换 |

传 `raw_body=true` 可跳过转换，直接输出原始 body。

## INDEX.md

自动生成的索引文件包含：
- 导出时间和文档统计
- 按 TOC 目录结构组织的文档树（带可点击链接）
- 无 TOC 时降级为平铺表格

## GRAPH.md

自动生成的引用关系文件包含：
- 文档间链接关系（谁引用了谁）
- 未解析的外部链接标注 ⚠️
- 引用统计（总数、已解析、未解析）

## 注意事项

- 大知识库（>100 篇）导出时间较长，建议设置较长的超时时间
- 图片下载 15 秒超时，超时自动降级为原链接
- 重复 URL 的图片只下载一次
- 输出文件使用文档标题命名，非法字符替换为 `_`
- 子目录中的文档引用图片使用相对路径（`../images/`）
- TOC 获取失败时降级为平铺导出（不影响导出流程）

## 使用示例

```
导出知识库 yehuoshun/my-docs 到 /tmp/backup/
导出时跳过图片下载以加快速度
```

## 调完干嘛

- 打开 `INDEX.md` 浏览目录结构
- 打开 `GRAPH.md` 查看文档引用关系
- 检查报告中 `images_failed` 数量，手动补下载失败的图片
- Markdown 文件可直接用 Obsidian/Typora/VSCode 打开

## 别干的事

- 不要并发导出同一个知识库，会导致重复文件
- 不要将 output_dir 设在语雀仓库内，避免循环

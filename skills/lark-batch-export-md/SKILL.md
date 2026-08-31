---
name: lark-batch-export-md
description: 批量将飞书云文档（docx）导出为本地 Markdown 文件，自动下载文档中的图片到 assets/ 目录并替换为本地相对路径。支持指定文件夹递归遍历所有子文档、单篇文档导出、保留目录结构、自动处理导出限频和异步任务。用于"把飞书文档批量导出成 markdown""导出飞书文档为 md""批量下载飞书文档为本地文件""飞书文档转 markdown"等需求。仅处理 docx 类型文档，不处理表格、幻灯片、多维表格。
---

# Lark Batch Export MD｜飞书文档批量导出 Markdown

## 前置条件
- `lark-cli` 已安装并完成登录授权（`lark-cli auth status` 可验证）
- 当前账号对目标文档有阅读和导出权限

## 快速使用

脚本路径：`scripts/batch_export_md.py`（相对于本技能目录）

### 1. 先 dry-run 看有哪些文档
```bash
python3 <skill_dir>/scripts/batch_export_md.py \
  --folder-url "https://xxx.feishu.cn/drive/folder/<folder_token>" \
  --output-dir ./exported \
  --dry-run
```

### 2. 实际导出（含图片自动下载）
```bash
python3 <skill_dir>/scripts/batch_export_md.py \
  --folder-url "https://xxx.feishu.cn/drive/folder/<folder_token>" \
  --output-dir ./exported
```

### 3. 单篇文档导出
```bash
python3 <skill_dir>/scripts/batch_export_md.py \
  --doc-url "https://xxx.feishu.cn/docx/<docx_token>" \
  --output-dir ./exported
```

## 常用参数

| 参数 | 说明 | 默认 |
|---|---|---|
| `--folder-url` | 飞书文件夹 URL（递归导出所有子文件夹中的 docx） | - |
| `--folder-token` | 飞书文件夹裸 token | - |
| `--doc-url` / `--doc-token` | 单篇文档导出 | - |
| `--output-dir` | 输出目录（必填） | - |
| `--delay` | 每篇导出间隔秒数，避免触发限频 | 8 |
| `--overwrite` | 覆盖已存在的 .md 文件（默认跳过已存在） | false |
| `--dry-run` | 只列出待导出文档，不实际执行 | false |
| `--max-retries` | 遇到限频时的最大重试次数 | 3 |
| `--include-wiki` | 同时导出 wiki 节点（默认只导出 docx） | false |
| `--skip-images` | 跳过图片下载和本地替换（默认自动下载图片到 assets/） | false |

## 输出结构

脚本保留源文件夹的相对目录结构，图片自动下载到每篇 .md 同级的 `assets/` 目录：
```
exported/
├── 2026/
│   └── 202608/
│       ├── 20260801-日记标题.md
│       ├── 20260802-日记标题.md
│       └── assets/
│           ├── 20260801-日记标题_img_001.png
│           └── 20260802-日记标题_img_001.jpg
├── 其他子文件夹/
│   └── 文档名.md
└── _export_report.json   ← 导出结果报告
```

## 图片处理（自动）

飞书文档导出为 markdown 时，图片默认是飞书内部 API URL（带时效 authcode，约 1 小时失效）。脚本**自动**完成以下处理：

1. 解析 .md 中所有 `![](https://internal-api-drive-stream.feishu.cn/...)` 图片链接
2. 立即下载图片到该 .md 同级的 `assets/` 目录
3. 图片命名为 `{文档名}_img_{序号}.{扩展名}`，避免同目录多篇文档冲突
4. 将 markdown 中的飞书内部 URL 替换为本地相对路径 `assets/{文件名}`
5. 同一 URL 出现多次时只下载一次，复用本地路径

**注意**：图片 authcode 有约 1 小时有效期，必须在导出后立即下载（脚本已内置此逻辑，无需手动操作）。如需跳过图片处理，加 `--skip-images` 参数。

## 常见问题

### 限频 (99991400 / rate_limit)
脚本会自动指数退避重试（60s → 120s → 240s）。如果批量文档很多，可以调大 `--delay`（如 15 或 20）降低触发概率。

### 权限不足 (1069902)
当前账号没有该文档的导出权限。需要文档 owner 授权，或在飞书中将文档移到当前账号可访问的文件夹。

### 异步导出超时
`+export` 内置 50 秒轮询。超时后脚本自动用 `+task_result` 续查并下载。如果续查也超时，会在报告中标记为失败，可单独重跑该篇。

### 图片下载失败
图片下载失败不影响 .md 导出，该图片链接会保留为飞书内部 URL。可在报告中查看失败数量，单独重跑该篇文档（加 `--overwrite`）。

### 文件夹中有非 docx 文件
脚本自动跳过 sheet、slides、bitable、file 等类型，只导出 docx。如需导出其他类型，用 `lark-cli drive +export` 单独处理（sheet→xlsx/csv，slides→pptx/pdf）。

## 验证清单

导出完成后检查：
- [ ] `_export_report.json` 中 failed 数量为 0（或已知原因）
- [ ] 抽查 2-3 篇 .md 文件，内容完整、格式正确
- [ ] 含图片的文档，确认 `assets/` 目录下有对应图片，且 .md 中引用的是 `assets/...` 相对路径
- [ ] 用 markdown 预览器打开 .md，确认图片正常显示
- [ ] 目录结构与源文件夹一致

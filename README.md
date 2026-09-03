# 让记录有下文｜豆包 + 飞书版

**Capture. Reflect. Practice.**

这是一个与 AI 协作的个人记录、回看与实践系统，也是 **豆包 + 飞书云盘** 路径的独立配置仓库。

在这条路径中：

- 豆包负责接住对话、执行 Skill 和触发计划任务；
- 飞书云盘负责长期保存 `journals/`、`notes/`、`reviews/`、`practices/` 中的材料和文章草稿；
- 本 GitHub 仓库只保存可安装的 Skills、计划任务 Prompt 和搭建说明，不保存参与者的私人记录。

## 包含的 Skills

- [capture-journal](skills/capture-journal/SKILL.md)：记录亲历事件、感受和身体经验，保存为 Markdown 文件，写入飞书云盘的 `journals/`。
- [capture-journal-feishudoc](skills/capture-journal-feishudoc/SKILL.md)：功能同上，但保存为飞书云文档（docx），支持插入图片。
- [capture-note](skills/capture-note/SKILL.md)：保存文章、播客、书、视频和对话等外部输入，写入 `notes/`。
- [weekly-review](skills/weekly-review/SKILL.md)：读取最近七天的日记记录和笔记，把回看档案保存到 `reviews/`、提出问题，并发现少量候选方向。
- [new-article](skills/new-article/SKILL.md)：围绕选定主题召回材料、追问表达，并持续更新文章草稿。
- [develop-practice](skills/develop-practice/SKILL.md)：把有重复、行动或反馈证据的线索发展为持续实践，并保存到 `practices/`。
- [bubble-breaker](skills/bubble-breaker/SKILL.md)：发现一个陌生资源，完成后再留下轻量记录。

## 开始使用

1. 创建或打开一个豆包智能体，挂载支持文件操作的飞书云盘插件，并完成认证授权。
2. 在飞书云盘准备 `journals/`、`notes/`、`reviews/` 与 `practices/`，或允许 Bot 在第一次使用时创建。
3. 打开准备使用的 `skills/<skill-name>/SKILL.md`，把完整内容安装为豆包 Skill。豆包没有 ChatGPT Project Settings，不要复制 ChatGPT 版本的配置。
4. 先安装并测试 `capture-journal` 或 `capture-note`：用一条不敏感内容确认豆包可以列出目标飞书目录、读取相关文件，并把结果上传回正确目录。
5. 只有返回真实的飞书文件链接，才算持久化成功。本地临时文件或一段模拟命令不算保存完成。
6. 基础读写确认成功后，再根据 [计划任务说明](scheduled-task-prompts/README.md)配置 `weekly-review` 或 `bubble-breaker`。

> 飞书允许同名文件夹并存。任何 Skill 创建目录前都必须完整列出父目录子项并精确匹配名称：1 个则复用，0 个才创建，多个则停止并消歧。

## 术语约定

- `journal entry`、`note`、`review` 和 `practice` 表示单个内容或概念。
- `journals/`、`notes/`、`reviews/` 和 `practices/` 表示飞书云盘中的真实目录；说明文字中的目录名始终使用小写复数、反引号和结尾斜杠。
- 飞书层级统一写成 `notes/ → {YYYY}/ → {YYYYMM}/`；`lark-cli` 命令中的 `"notes"` 仍是实际文件夹名称，因此不添加斜杠。
- Skill 名称使用小写连字符并放在反引号中，例如 `capture-note` 和 `weekly-review`。

## 仓库目录

```text
.
├── skills/
│   ├── capture-journal/
│   ├── capture-journal-feishudoc/
│   ├── capture-note/
│   ├── weekly-review/
│   ├── new-article/
│   ├── develop-practice/
│   └── bubble-breaker/
├── scheduled-task-prompts/
│   ├── weekly-review.md
│   └── break-bubble.md
└── README.md
```

## ChatGPT + GitHub 版本

如果希望把个人记录保存在 GitHub，并直接通过 ChatGPT 使用，请使用独立仓库：[capture-reflect-practice](https://github.com/sunling/capture-reflect-practice)。

本项目采用 [MIT License](LICENSE)。

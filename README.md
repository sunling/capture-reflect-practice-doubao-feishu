# 让记录有下文｜豆包 + 飞书版

**Record. Reflect. Practice.**

这是一个与 AI 协作的个人记录、回看与实践系统，也是 **豆包 + 飞书云盘** 路径的独立配置仓库。

在这条路径中：

- 豆包负责接住对话、执行 Skill 和触发计划任务；
- 飞书云盘负责长期保存 Journal、Inputs、Reviews、Practices 和文章草稿；
- 本 GitHub 仓库只保存可安装的 Skills、计划任务 Prompt 和搭建说明，不保存参与者的私人记录。

## 包含的 Skills

- [capture-journal](skills/capture-journal/SKILL.md)：记录亲历事件、感受和身体经验，写入飞书云盘的 `journal/`。
- [capture-input](skills/capture-input/SKILL.md)：保存文章、播客、书、视频和对话等外部输入，写入 `inputs/`。
- [weekly-review](skills/weekly-review/SKILL.md)：读取最近七天的 Journal 和 Inputs，保存 Review、提出问题，并发现少量候选方向。
- [new-article](skills/new-article/SKILL.md)：围绕选定主题召回材料、追问表达，并持续更新文章草稿。
- [develop-practice](skills/develop-practice/SKILL.md)：把有重复、行动或反馈证据的线索发展为持续 Practice。
- [bubble-breaker](skills/bubble-breaker/SKILL.md)：发现一个陌生资源，完成后再留下轻量记录。

## 开始使用

1. 创建或打开一个豆包智能体，挂载支持文件操作的飞书云盘插件，并完成认证授权。
2. 在飞书云盘准备 `journal/`、`inputs/`、`reviews/` 与 `practices/`，或允许 Bot 在第一次使用时创建。
3. 打开准备使用的 `skills/<skill-name>/SKILL.md`，把完整内容安装为豆包 Skill。豆包没有 ChatGPT Project Settings，不要复制 ChatGPT 版本的配置。
4. 先安装并测试 `capture-journal` 或 `capture-input`：用一条不敏感内容确认豆包可以列出目标飞书目录、读取相关文件，并把结果上传回正确目录。
5. 只有返回真实的飞书文件链接，才算持久化成功。本地临时文件或一段模拟命令不算保存完成。
6. 基础读写确认成功后，再根据 [计划任务说明](scheduled-task-prompt/README.md)配置 Weekly Review 或 Bubble Breaker。

> 飞书允许同名文件夹并存。任何 Skill 创建目录前都必须完整列出父目录子项并精确匹配名称：1 个则复用，0 个才创建，多个则停止并消歧。

## 仓库目录

```text
.
├── skills/
│   ├── capture-journal/
│   ├── capture-input/
│   ├── weekly-review/
│   ├── new-article/
│   ├── develop-practice/
│   └── bubble-breaker/
├── scheduled-task-prompt/
│   ├── weekly-review.md
│   └── break-bubble.md
└── README.md
```

## ChatGPT + GitHub 版本

如果希望把个人记录保存在 GitHub，并直接通过 ChatGPT 使用，请使用独立仓库：[record-reflect-practice](https://github.com/sunling/record-reflect-practice)。

本项目采用 [MIT License](LICENSE)。

---
name: develop-practice
description: 将 Daily 中反复出现、已经行动或获得反馈的线索，发展为最小可持续 Practice；也用于更新现有 Practice 的使命、当前一轮、行动边界与反馈记录。用户说“把这件事变成长期实践”“从记录里发展一个项目”“继续推进这个实践”时使用。
---

# Develop Practice｜把记录发展成实践

## 目标

把已经在生活里发生的事情整理成可以持续行动、观察和接收反馈的 Practice，而不是从一个孤立念头制造项目。

```text
Daily 中出现线索
→ 确认重复、行动或反馈证据
→ 定义最小实践
→ 做一轮真实行动
→ 记录反馈
→ 决定继续、调整、暂停或结束
```

## 开始前读取

- `PROFILE.md` 中的偏好、时区和隐私边界（如可用）；
- 支持该线索的少量 `daily/journal/` 和 `daily/inputs/`；
- 同名或近义 Practice 已存在时，读取其 `mission.md`、`current.md` 和当前任务必要的材料。

不要扫描全部历史。若证据不足，说明还缺少什么，让线索继续留在 Daily。

## 证据门槛

创建新 Practice 前，至少确认以下一项：

1. **重复**：同一需要、问题或主题出现在多条记录中；
2. **行动**：用户已经做过至少一次真实尝试；
3. **反馈**：已经得到自身感受、行为变化、他人回应或环境结果。

用日期和飞书云盘中的相对路径标明证据。不要把计划写成已经发生，也不要从短期情绪推断长期人格。

## 工作流

1. 确认用户想继续行动，而不只是保存或回看。
2. 检查是否已有同名或近义 Practice；有则更新，不重复创建。
3. 与用户确认名称、存在原因、首先服务谁、长期边界和当前一轮。
4. 只设计一个足够小、能够真实完成的本轮行动。
5. 写入或更新最小文件结构。
6. 说明下一次回看时间，以及这一轮明确不做什么。

## 最小文件结构

```text
practices/{practice-name}/
├── mission.md
├── current.md
└── drafts/      # 只有输出练习开始产生文章时才增加
```

目录名使用小写英文和连字符，不使用空格、`v2` 或 `final`。

如果这是会持续产生文章的输出练习，仍先建立 `mission.md` 和 `current.md`；第一篇文章开始起草时，再由 `new-article` 增加 `drafts/` 并管理其中草稿。单篇文章主题不能替代 Practice 名称。

### `mission.md`

```md
# {实践名称}

## 为什么存在
{它回应的真实需要}

## 服务谁
{首先服务的对象}

## 做什么
{长期会做的事}

## 不做什么
{长期边界}

## 来源
- {日期与相关 Daily 相对路径}
```

### `current.md`

```md
# 当前一轮

## 当前假设
{这一轮想验证什么}

## 本轮行动
- **触发条件**：{具体场景}
- **具体行动**：{足够小、可观察的行动}
- **记录方式**：{低摩擦记录方式}

## 如何观察反馈
{身体感受、行为变化、他人回应或环境结果}

## 下次回看
{日期或周期}

## 本轮不做
{这一轮明确排除的事}
```

只保留有真实材料的部分，不用空话填满模板。

## 更新现有 Practice

- 优先更新 `current.md`；使命、对象或长期边界未改变时，不重写 `mission.md`。
- 先记录已经发生的行动与反馈，再决定下一轮。
- 每轮只保留一个主要行动。
- 需要历史时先遵循现有约定；没有约定时，不自行建立复杂归档。
- 允许暂停或结束。停止不等于失败。

## 飞书 Drive 执行与存储

AI 对话环境中的本地文件通常是临时的，长期内容必须上传到飞书 Drive。

1. 使用相对路径准备 `./mission.md` 与 `./current.md`；不要使用绝对路径。
2. 在 `practices` 下定位或创建 `{practice-name}` 子目录。

   > **⚠️ 关键警告**：飞书 Drive 允许同名文件夹并存。不能因为搜索不到预期结果就直接创建。先定位目标 `practices`，再完整列出其子项并精确匹配目录名。

   ```bash
   lark-cli drive +search --query "practices" --doc-types folder --format json
   lark-cli drive files list --params '{"folder_token":"{practices文件夹token}","page_size":200}' --format json
   ```

   - 搜索到多个同名 `practices` 时，根据目标系统根目录或既有配置消歧；无法确定时让用户确认，不创建新的根目录；
   - 完整处理分页后，在 `data.files` 中筛选 `type == "folder"` 且 `name == "{practice-name}"`；
   - 找到 1 个 → 复用其 token，并读取已有 `mission.md` 与 `current.md`；
   - 找到 0 个 → 确认证据门槛后才创建；
   - 找到多个 → 不再创建，根据既有文件消歧；仍无法确定时让用户选择。

3. 新建时把两个文件上传到该子目录，并保存各自的 `file_token` 和链接。
4. 更新时先下载现有文件；默认只更新 `current.md`。使用已有 `file_token` 覆盖上传，保留原链接。
5. 本地创建但没有上传不算完成。
6. 成功后只报告飞书目录、文件名和链接，不重复完整正文。

仅在精确匹配 `{practice-name}` 的目录数量为 0 时，才执行创建：

```bash
lark-cli drive +create-folder --name "{practice-name}" --folder-token "{practices文件夹token}"
lark-cli drive +upload --file ./mission.md --folder-token "{practice子目录token}"
lark-cli drive +upload --file ./current.md --folder-token "{practice子目录token}"
lark-cli drive +upload --file ./current.md --file-token "{current.md 的 file_token}"
```

`create-folder` 只能在精确匹配数量为 0 时执行。

## 隐私与事实边界

- 只读取和写入当前 Practice 必要的材料。
- 涉及第三方、身体、医疗、关系或工作信息时，遵循 `PROFILE.md`。
- 不编造使命、受众、行动、反馈或承诺。
- 公开表达前，单独标记需要匿名化、授权或删除的细节。

## 完成检查

- 是否存在重复、行动或反馈证据？
- 是否链接了具体 Daily？
- 是否检查并避免重复 Practice？
- 是否只保留一个足够小的本轮行动？
- 是否写清本轮不做什么和下次回看时间？
- 是否已上传到飞书 Drive，而不只保存在临时本地文件？
- 是否把继续、调整或结束的决定留给用户？

## 完成后的回复

简要说明：

1. Practice 目录及创建或更新的文件；
2. 采用的证据；
3. 当前一轮行动和下次回看时间；
4. 仍需用户确认的事实或边界。

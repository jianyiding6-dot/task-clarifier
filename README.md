# task-clarifier · 任务澄清与执行闭环

一个需要你主动调用的中文 Codex Skill（技能：一组可重复使用的工作指令）。适合不熟悉编程和提示词、希望用 Codex 完成实际任务的成年人。

你可以先说一个模糊目标。它会预填已知信息，每轮最多问三个关键问题，逐步明确结果、材料和标准；满足开工条件后，在授权范围内执行，再检查成果并按反馈局部修改。适用于写作、分析、教学、方案设计和代码项目。

**`v0.1.0-beta.1` 是预发布版。** 完整的安装后真实多轮 Codex 对话验证尚未完成。它能帮助组织任务，但不能保证理解、判断或成果始终正确。

仓库：[jianyiding6-dot/task-clarifier](https://github.com/jianyiding6-dot/task-clarifier) · [版本发布页](https://github.com/jianyiding6-dot/task-clarifier/releases/tag/v0.1.0-beta.1) · [下载 ZIP](https://github.com/jianyiding6-dot/task-clarifier/archive/refs/tags/v0.1.0-beta.1.zip)

## 安装前

需要支持本地 Skill 的 Codex 环境。本 Skill 没有运行脚本、Python/Node 依赖或必装外部服务；具体任务需要的文件、工具和权限由你的任务决定。

安装策略依据 [OpenAI 官方 Skill 文档](https://learn.chatgpt.com/docs/build-skills)，核对日期为 2026-09-21。个人安装目录采用 `~/.agents/skills/`，也可在特定项目放入 `.agents/skills/`。`~` 表示你的用户主目录；Windows 对应 `%USERPROFILE%`。不要把整个仓库或 ZIP 文件直接当作技能目录。

## 路径一：下载 ZIP 后手动安装

从 [v0.1.0-beta.1 版本发布页](https://github.com/jianyiding6-dot/task-clarifier/releases/tag/v0.1.0-beta.1) 的 Assets（附件）点击 **Source code (zip)**，或使用上面的“下载 ZIP”。这是 GitHub 按版本生成的完整源码包；解压后的外层文件夹通常是 `task-clarifier-0.1.0-beta.1`，技能位于其中的 `skills/task-clarifier/`。

1. 下载并解压 ZIP。进入解压得到的仓库文件夹，找到 `skills/task-clarifier/`。
2. 把这个 `task-clarifier` **整个文件夹**复制到你的技能目录。首次安装时，如果同名目录已存在，先按下方“更新”步骤处理，不直接覆盖。
3. 安装后的关键路径应是：

   - macOS / Linux：`~/.agents/skills/task-clarifier/SKILL.md`
   - Windows：`%USERPROFILE%\.agents\skills\task-clarifier\SKILL.md`
   - 仅当前项目可用：`项目目录/.agents/skills/task-clarifier/SKILL.md`

4. 检查同级的 `LICENSE`、`agents/`、`references/` 也被复制；不能只复制 `SKILL.md`。开启下一轮对话，若没有出现则重启 Codex。

不要在技能目录里再嵌套一层 `skills/task-clarifier`；不要同时在多个扫描位置安装同名副本。若安装器之前把它放在 `~/.codex/skills/` 或自定义的 `CODEX_HOME/skills/`，先确认现有位置，再选择更新位置。

## 路径二：让 Codex 的技能安装器安装

如果你的环境提供 `$skill-installer`，在 Codex 中粘贴下面这段话（这是对 Codex 的请求，不是终端命令）：

```text
$skill-installer
请从 https://github.com/jianyiding6-dot/task-clarifier 安装技能。
版本：v0.1.0-beta.1
仓库内路径：skills/task-clarifier
目标位置：我的用户主目录下 .agents/skills/task-clarifier
如果同名技能已存在，先报告实际位置并保留我的修改，不要直接覆盖。
```

也可提供 [固定版本的技能目录链接](https://github.com/jianyiding6-dot/task-clarifier/tree/v0.1.0-beta.1/skills/task-clarifier)，明确要求安装这个版本。

安装器需要网络访问，可能触发当前环境的权限提示。安装器版本可能采用 `CODEX_HOME/skills`（未自定义时通常是 `~/.codex/skills`）作为默认位置；可以明确要求安装到官方个人目录 `~/.agents/skills`。以安装器实际报告的目录为准，后续就在同一位置更新。若该环境没有安装器，使用路径一。

## 开始使用

在 Codex 输入 `$` 并选择 `task-clarifier`，或使用以下示例。支持其他选择界面的版本可直接从技能列表选择“任务澄清与执行闭环”。

```text
$task-clarifier
我想把读书笔记整理成能反复使用的内容，但不知道做成什么。请先帮我澄清。
```

```text
$task-clarifier
请把下面的工作记录整理成给同事看的周报，聊天里交付，300字以内。
只用我给的事实，保留问题与下一步计划。信息齐全就执行。
（接着写你的工作记录）
```

直接用日常语言回答即可，不用严格照表格填。可以说“不清楚”“给我选项”“由你建议”。你已提供的信息应被复用，不必重复输入。

| 你说 | 应有行为 |
| --- | --- |
| 只澄清，等我确认 | 信息齐全也先停在任务卡，直到你明确允许执行 |
| 信息齐全就执行 | 通过开工检查后执行已授权工作 |
| 显示完整任务卡 | 展示已知、未知、边界、权限和验收标准；不会自动解除等待模式 |
| 我不清楚，给我选项 | 给用途和取舍清楚的具体方向 |
| 保留其他部分，只修改第二段 | 只处理相关问题，并检查保留内容没有改变 |
| 导出任务卡，我要在新对话继续 | 导出可接续信息；新对话仍要核实材料与权限 |

它默认不介入未选择本 Skill 的普通提问，配置见 [openai.yaml](skills/task-clarifier/agents/openai.yaml)。其他已安装技能或全局设置仍可能影响 Codex 的行为。

## 六环怎样工作

1. **明确结果**：区分长期目标和这轮交付，明确给谁用、以什么形式交付。
2. **补齐依据**：先读取已授权且可访问的材料，再问关键缺口。
3. **定义标准**：把“专业、好看、有逻辑”变成可检查的要求，明确优先级、禁区和保留项。
4. **按范围执行**：检查交付物、核心材料、边界权限、验收办法、冲突、工具环境，然后展示简短任务卡与计划并执行。
5. **对照验收**：成果出现后检查标准，说明通过、未通过及未验证项。
6. **精准纠偏**：区分未达标、歧义和新需求，保留满意部分，修改后复验。

前三环可以合并，也可以分几轮。不能在关键问题仍未回答时自作主张制作最终成果；也不应在信息充分时无限澄清。发布、付款和删除重要数据等操作仍需要相应授权，不能因为“信息齐全”就跳过权限要求。

## 文件结构

```text
task-clarifier/
├── README.md
├── LICENSE
├── CHANGELOG.md
├── .gitignore
├── skills/
│   └── task-clarifier/
│       ├── SKILL.md
│       ├── LICENSE
│       ├── agents/openai.yaml
│       └── references/
│           ├── forms.md
│           └── examples.md
└── tests/
    ├── behavior-cases.md
    └── check-results.md
```

安装时只需复制 `skills/task-clarifier/`；仓库层 README 和测试记录供阅读、维护与检查。不要把私人任务卡、材料或聊天记录保存进这个公共仓库。

## 检查与限制

详细用例和实际状态见 [行为测试记录](tests/behavior-cases.md) 与 [文件及环境检查记录](tests/check-results.md)。静态检查、编写的模拟示例、独立代理实验和真实 Codex 对话测试分别记录。静态文件正确不等于模型行为已验证，生成文件不等于任务已验收。

首次版本在完整多轮实测完成前保持预发布。跨平台安装和不同模型可能有差异，未实际检查的环境不会宣称支持已经过验证。

## 更新与卸载

**使用者更新：** 先阅读 [CHANGELOG.md](CHANGELOG.md) 和新版本测试状态；备份现有 `task-clarifier` 到技能扫描目录以外，避免旧副本继续被识别。下载新版本，核对来源，再用新的完整 `skills/task-clarifier/` 替换同一安装位置的旧目录，避免逐文件合并遗留过期规则。重新开启对话，必要时重启 Codex。不要在新目录里放旧的任务卡。

**通过安装器更新：** 安装器可能拒绝已有同名目录。先让 Codex 确认实际安装位置和备份情况，再按你选择的新版本重新安装；不要自行批量删除其他技能。固定版本安装不会自动跟随 GitHub 更新。

**卸载：** 只把实际安装位置下的 `task-clarifier` 文件夹移出所有技能扫描目录，或在确认不需要保留后删除该文件夹。不要删除整个 `skills` 目录。重新开启对话，若仍显示则重启 Codex。

**维护者发新版：** 修改通用指令或参考文件，更新变更记录，重跑静态检查和受影响的 A–H 行为用例；清楚记录未验证项。只发布审查后的项目文件，更新版本号和 README 安装示例，创建对应 Release 与 ZIP，再用无登录方式下载核验。不要提交私人任务记录或凭据。

## 许可证与署名

采用 [MIT 许可证](LICENSE)，Copyright (c) 2026 jianyiding6-dot。复制、修改或分发时请保留版权和许可声明。技能子目录内也附带同一份许可证，方便安装器仅下载该目录时保留许可。

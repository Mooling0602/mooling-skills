# mooling-skills

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![](https://img.shields.io/badge/powered_by-dsh-4D6BFE?style=flat-square&logo=deepseek&logoColor=white)](https://github.com/deepseek-ai/deepseek-harness)

[English](README_en_US.md) | 中文

个人使用的 AI Agent 技能集合，遵循 [Agent Skills](https://agentskills.io/specification) 开放规范。

# 结构

```
skills/
├── ddgs/SKILL.md           # 联网搜索
├── mcdr/SKILL.md           # MCDReforged 插件开发
└── request-sudo/SKILL.md   # GUI 密码框提权
```

每个技能是 `skills/` 下的一个目录，目录名即技能名（kebab-case），内含一份带 YAML frontmatter 的 `SKILL.md`。技能需要附带脚本、参考文档或资源文件时，放在同目录下的 `scripts/`、`references/`、`assets/` 中。

# 安装

## skills CLI

```sh
npx skills add Mooling0602/mooling-skills
```

加 `--skill <名称>` 只装指定技能，加 `-g` 装到用户级目录而非当前项目。用 `--list` 可以先看清单再决定。

## 直接复制

技能本身就是完整目录，把需要的技能目录拷进对应工具的 skills 目录即可：

```sh
git clone https://github.com/Mooling0602/mooling-skills
cp -r mooling-skills/skills/ddgs ~/.dsh/skills/
```

常见工具的 skills 目录：

| 工具 | 用户级路径 |
| --- | --- |
| DeepSeek Harness | `~/.dsh/skills/` |
| Claude Code | `~/.claude/skills/` |
| Codex / OpenCode / Cursor 等 | `~/.agents/skills/` |

## 对本仓库内的技能

如果项目与 `skills/` 目录同级，也可以通过项目的 `.dsh/skills` 或 `.agents/skills` 软链接引入，改动会实时生效。

# 编写技能

`SKILL.md` 的 frontmatter 至少要包含两个字段：

```yaml
---
name: my-skill
description: 一句话说明这个技能做什么，以及在什么情况下该使用它。
---
```

- `name` 只能是小写字母、数字和连字符，不能以连字符开头或结尾，且必须与所在目录同名。
- `description` 会直接进入模型的技能目录，用于判断是否触发该技能。写清楚「什么时候用」比写「是什么」更重要。

正文是模型在技能被激活后读到的完整指令。建议控制在 5000 token 以内，把大段参考材料拆到 `references/` 中按需读取。

安装后的技能文件可以去除其中的「安装」部分内容，因为已经完成了格式转换。另外可以在 frontmatter 的 `metadata.source` 中保留技能文件的源 URL，方便再次从 GitHub 读取并更新。

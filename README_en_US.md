# mooling-skills

A personal collection of AI Agent skills, following the open [Agent Skills](https://agentskills.io/specification) specification.

# Layout

```
skills/
├── ddgs/SKILL.md           # Web search
├── mcdr/SKILL.md           # MCDReforged plugin development
└── request-sudo/SKILL.md   # Elevation via GUI password dialog
```

Each skill is a directory under `skills/`. The directory name is the skill name (kebab-case), and it contains a `SKILL.md` with YAML frontmatter. When a skill needs bundled scripts, reference docs, or assets, they live in `scripts/`, `references/`, or `assets/` inside the same directory.

# Install

## skills CLI

```sh
npx skills add Mooling0602/mooling-skills
```

Add `--skill <name>` to install a single skill, and `-g` to install into the user-level directory instead of the current project. Use `--list` to inspect the catalog before installing.

## Copy directly

A skill is already a self-contained directory, so you can copy the ones you want into the skills directory of your tool:

```sh
git clone https://github.com/Mooling0602/mooling-skills
cp -r mooling-skills/skills/ddgs ~/.dsh/skills/
```

Skills directories for common tools:

| Tool | User-level path |
| --- | --- |
| DeepSeek Harness | `~/.dsh/skills/` |
| Claude Code | `~/.claude/skills/` |
| Codex / OpenCode / Cursor, etc. | `~/.agents/skills/` |

## Within this repository

When a project sits next to the `skills/` directory, it can also bring the skill in through a symlink under the project's `.dsh/skills` or `.agents/skills`, so edits take effect immediately.

# Writing a skill

The `SKILL.md` frontmatter needs at least two fields:

```yaml
---
name: my-skill
description: One sentence on what the skill does and when it should be used.
---
```

- `name` may only contain lowercase letters, digits, and hyphens; it must not start or end with a hyphen, and it must match the directory name.
- `description` goes straight into the model's skill catalog and decides whether the skill triggers. Writing *when to use it* matters more than writing *what it is*.

The body is the full instruction the model reads once the skill activates. Keep it under roughly 5000 tokens and push bulky reference material into `references/` to be read on demand.

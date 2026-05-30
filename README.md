# claude-playground

Custom skills, workflows, and experiments for [Claude Code](https://claude.ai/code).

## Skills

| Skill | Description |
| --- | --- |
| `deepdive` | Generate book-quality Obsidian vaults on any technical topic — progressive difficulty, Mermaid diagrams, case studies, and interlinked WikiLinks. |

## Usage

Place skills in `~/.claude/skills/` (user-scoped) or `.claude/skills/` (project-scoped). Claude Code discovers them automatically.

To use a skill, type `/<skill-name>` in the Claude Code prompt.

## Structure

```
.
├── skills/          # Custom Claude Code skills
│   └── deepdive/    # Deep-dive learning guide generator
└── workflows/       # Reusable workflows (coming soon)
```

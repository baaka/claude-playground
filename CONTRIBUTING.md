# Contributing to claude-playground

Thanks for considering a contribution. This repo collects custom skills, workflows, and tooling for [Claude Code](https://claude.ai/code). Bug fixes, new skills, workflow improvements, and documentation are all welcome.

## Table of contents

- [Code of conduct](#code-of-conduct)
- [What to contribute](#what-to-contribute)
- [Getting started](#getting-started)
- [Skill structure](#skill-structure)
- [Quality requirements](#quality-requirements)
- [Pull request process](#pull-request-process)
- [Style guide](#style-guide)

## Code of conduct

Be respectful. Assume good faith. If someone suggests an improvement, engage with the idea rather than reacting defensively. If you see problematic behavior, open an issue or contact a maintainer.

## What to contribute

| Area | Ideas |
|------|-------|
| **New skills** | `/my-skill` — a reusable Claude Code skill with clear triggers, instructions, and reference files |
| **Skill improvements** | Better prompts, more robust quality gates, additional templates, CVE references |
| **Workflows** | Reusable multi-step automation patterns, CI integration, or project bootstrapping scripts |
| **Documentation** | README updates, usage examples, or supplementary guides |
| **Bug fixes** | Broken links, incorrect frontmatter, script errors in quality-gate commands |

Not sure if something fits? Open an issue to discuss before writing code.

## Getting started

1. **Fork** the repo and clone your fork.
2. **Create a branch** off `main` with a descriptive name:
   ```
   skill/<skill-name>      # new skill
   fix/<description>       # bug fix
   docs/<description>      # documentation
   ```
3. **Make your changes.** Follow the conventions below.
4. **Test locally.** Place the skill in `.claude/skills/` and invoke it in Claude Code to confirm it works end-to-end.
5. **Push** and open a pull request against `main`.

## Skill structure

Every skill lives in `skills/<name>/` and must include:

```
skills/<name>/
├── SKILL.md                    # Required: YAML frontmatter + instructions
└── references/                 # Optional: templates, examples, schemas
    └── ...
```

### SKILL.md frontmatter

```yaml
---
name: <kebab-case-name>
description: <when to trigger + what the skill produces>
argument-hint: "<hint shown to the user>"
version: <semver>
---
```

- `name` — Must match the directory name (`skills/my-skill/` → `name: my-skill`).
- `description` — A single sentence covering both **when to invoke the skill** (trigger phrases) and **what it does**. Claude uses this to auto-discover skills.
- `argument-hint` — Shown in the UI when the user types `/<skill-name>`. Keep it under one line.
- `version` — Follow [SemVer](https://semver.org). Increment when changing behavior.

### Body of SKILL.md

The body after the frontmatter is the system prompt that Claude receives when the skill is invoked. Write it as direct instructions to the model:

- Start with a one-line role definition ("You are building an…").
- List rules as numbered, imperative statements. Be exact — the model follows these literally.
- Include quality gates with specific bash commands to run (e.g., `grep`, `find`, `sed`) so the model can self-audit.
- Reference files in `references/` by path so the model reads them (e.g., "Read `references/file-templates.md` for the complete templates").

### Reference files

Place templates, schemas, examples, or lookup tables in `references/`. Keep them as Markdown or structured data files. Reference files are read by the model at runtime — the skill prompt tells the model when and how to use them.

## Quality requirements

A new skill must:

1. **Trigger correctly.** Claude should discover and invoke it for the described trigger phrases. The `description` field is the only discovery mechanism — make it specific enough to avoid false matches.

2. **Produce reproducible output.** Given the same inputs, the skill should produce consistent structure and quality.

3. **Include quality gates.** When the skill produces output (files, code, structured data), include verification commands the model can run to self-check correctness.

4. **Stay focused.** A skill does one thing well. If you're adding a second personality or an unrelated feature, it belongs in a separate skill.

5. **Respect context limits.** Reference files should be concise. Don't include unnecessary background that the model can derive from names and structure.

## Pull request process

1. **Keep PRs small.** One skill or fix per PR. Large, multi-topic PRs are harder to review.
2. **Write a clear description.** Explain what the change does and why. For new skills, include example output or a sample invocation.
3. **Update the README.** If you add a new skill, add a row to the skills table.
4. **Self-review first.** Run through your own PR as a reviewer would — check for typos, broken paths, and frontmatter validity.
5. **Squash commits** when merging. The commit message to `main` should be a single sentence describing the change (e.g., `add review skill: PR review with code-quality checks`).

PRs that don't follow the conventions above will get a friendly nudge before review.

## Style guide

- **File names:** kebab-case (`my-skill-name.md`).
- **YAML frontmatter:** All keys lowercase, no quotes unless needed, valid YAML.
- **Markdown:** Use reference-style links for URLs within paragraphs. Prefer tables over bullet lists for structured comparisons.
- **Shell commands in quality gates:** Prefer portable POSIX-compatible commands. Avoid bashisms unless necessary.
- **No commented-out code.** Delete it — git history exists for a reason.
- **One sentence per line** in prose files (the [semantic line break](https://sembr.org) convention) for cleaner diffs.

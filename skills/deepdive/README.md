# `/deepdive` — Obsidian Knowledge Base & PDF Generator

A Claude Code skill that generates comprehensive, multi-file Obsidian vaults and printable PDFs for any technical topic. One slash command produces a fully linked knowledge base with progressive difficulty, Mermaid diagrams, a continuous case study, and CVE or post-mortem deep dives.

## What It Produces

A directory of 40-80 Markdown files organized into a structured Obsidian vault, or a single printable PDF, or both — you choose at the start of each session.

### When You Choose "Obsidian Vault"

```
<topic-slug>-vault/
├── 00-Meta/          Glossary, cheat sheets, learning path, interview prep
├── 01-Foundations/   Prerequisite concepts and mental models
├── 02-<Subtopic>*/   Core topic sections, each with a hub note
├── 0X-Security/      Risks, CVEs, mitigations (or domain-equivalent)
├── 0Y-Integration/   Practical implementation guides
└── 0Z-Case-Study/    Fictional company evolution across 5-10 phases
```

Every file includes YAML frontmatter with `title`, `tags`, `difficulty`, and `related` fields. Files are cross-linked with `[[WikiLinks]]`. Hub notes serve as navigation maps; detail notes carry full explanations. No file is an orphan.

### When You Choose "PDF"

All content is written as Markdown files first, then concatenated and converted to a single PDF via `pandoc` + `xelatex`. The PDF includes a table of contents, page breaks between sections, and reasonable typography. Wiki links are converted to plain text. Mermaid diagrams render as code blocks (a known pandoc limitation).

## Installation

### Option A: Project-scope (this repo)

Clone this repository and run Claude Code inside it. The skill registers automatically from `skills/deepdive/SKILL.md`.

```bash
git clone <this-repo>
cd claude-playground
claude
```

### Option B: Personal (all projects)

```bash
mkdir -p ~/.claude/skills/deepdive
cp skills/deepdive/SKILL.md ~/.claude/skills/deepdive/SKILL.md
```

### Option C: Another project

```bash
mkdir -p <other-project>/.claude/skills/deepdive
cp skills/deepdive/SKILL.md <other-project>/.claude/skills/deepdive/SKILL.md
```

Only `SKILL.md` is required for the skill to work. The `references/` directory contains templates and examples that Claude reads during execution — keep them if you want the full template fidelity, skip them for a minimal install.

## Usage

```
/deepdive <technical topic with specific tools>
```

### Examples

```bash
/deepdive OAuth 2.0, OIDC, and SAML with Keycloak Architecture

/deepdive Docker and Kubernetes Production Hardening and Isolation

/deepdive Kafka Event-Driven Microservices and Failure Recovery
```

### What Happens

1. **Phase 0 — Format choice + Blueprint.** Claude asks whether you want an Obsidian vault, PDF, or both. Then it analyzes the topic, designs a fictional company, and presents a Map of Content showing every file with its difficulty level. You review and approve before anything is written.

2. **Phases 1-5 — Generation.** Claude writes files in order: foundations first, then core topics, then security/integration, then case study, then meta resources. Independent sections are written in parallel for speed.

3. **Phase 6 — Audit.** Claude runs link resolution, frontmatter checks, and orphan detection. Broken links are fixed. A summary is reported.

4. **Phase 7 — PDF (optional).** If you chose PDF or Both, Claude concatenates all files, strips wiki links, and runs pandoc to produce a single PDF.

## Key Design Decisions

- **MOC first.** A full file listing is presented for your approval before any files are created. You can add, remove, or reorganize before generation starts.
- **Format choice up front.** You pick Obsidian, PDF, or both before any work begins — no rework.
- **Fictional company, real incidents.** Every concept is grounded in a fictional company's growth story. Security and reliability topics reference real CVEs and post-mortems with actual CVE numbers.
- **Never pseudocode.** Configuration examples use real YAML, JSON, SQL, and CLI commands. Sequence diagrams use Mermaid.
- **Beginner to expert.** Files are explicitly tagged `beginner`, `intermediate`, or `advanced` and ordered into a learning path.
- **No orphan notes.** Every file links to prerequisites and is linked from at least one other file.

## Files in This Skill

| File | Purpose |
|------|---------|
| `SKILL.md` | Main skill body — rules, analysis protocol, execution order (311 lines) |
| `references/file-templates.md` | Complete Markdown templates for hub notes, detail notes, case study phases, and meta files |
| `references/example-moc.md` | Concrete MOC example using the OAuth/IAM topic as a reference |

## Requirements

- Claude Code (any recent version)
- The output is an Obsidian vault, but Obsidian itself is not required — the files work in any Markdown editor
- For PDF output: `pandoc` and a LaTeX engine (`basictex` on macOS, `texlive-xetex` on Linux)

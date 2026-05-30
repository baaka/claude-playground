---
name: deepdive
description: This skill should be used when the user asks to "deep dive", "create a deep dive", "build an Obsidian vault", "create a learning guide", "make a comprehensive guide", or wants book-quality documentation for any technical topic. Produces a structured multi-file Obsidian vault or a single PDF with progressive difficulty, Mermaid diagrams, real-world case studies, CVE deep-dives, and interlinked WikiLinks.
argument-hint: "<technical topic with specific tools, e.g. PostgreSQL query optimization>"
version: 1.0.0
---

# Deep Dive Generator

You are building an **Obsidian knowledge base** that teaches a technical topic from beginner to expert level. The topic comes from `$ARGUMENTS` — the user's full input after `/deepdive`. Treat `$ARGUMENTS` as the subject matter to cover.

The reader is a software engineer with basic experience but no prior knowledge of the specific topic. Every explanation must connect to practical system design.

## Must Follow These Rules

This is not optional. You must follow every rule below.

1. **Obsidian vault format.** Output multiple Markdown files organized in folders. Never produce one large document. Every file focuses on a single concept.

2. **YAML frontmatter on every file.** Each file starts with `---`, then `title`, `tags`, `difficulty` (beginner, intermediate, or advanced), and `related` (list of linked concepts). Then `---`.

3. **WikiLinks for all cross-references.** Use `[[like-this]]` for every link between files. If concept A is explained in its own file, link to it — never repeat the full explanation. Prefer linking over repetition.

4. **MOC first, files second.** Before writing any content files, present a Map of Content (MOC) — a single Markdown message showing the fictional company, the primary tool, the complete folder tree with every single file named (not just folders), the difficulty level for each file, the case study phase table, and the total file count. Ask the user to confirm. Do not write a single file until they approve.

5. **Hub-and-spoke navigation.** Some notes are hubs — they link to all subtopics but contain no deep explanations themselves. Others are detail notes with full content. Every file must have a `## Navigation` section at the end with **Prerequisites**, **Related Concepts**, and **Used In** lists.

6. **No orphan notes.** Every file must have at least: 1 prerequisite link, 2 related concept links, and 1 downstream usage link. If you cannot meet this, the structure needs redesigning.

7. **Bidirectional linking.** If file A links to file B, file B must reference file A somewhere. The vault is a graph, not a tree.

8. **Progressive difficulty.** Files progress from `difficulty: beginner` to `difficulty: advanced`. The learning path has a clear order.

9. **Practical, not theoretical.** Every concept includes a real-world scenario, a failed naive approach, and the correct solution. Include specific configuration examples (real YAML, JSON, SQL, CLI commands — never pseudocode), real request/response samples, decoded data structures, and terminal output when relevant. Concepts must be grounded in operational reality.

10. **Use Mermaid diagrams.** Include sequence diagrams, architecture diagrams, and flow charts wherever they clarify a concept. Use ` ```mermaid ` code blocks.

## How to Analyze the Topic

When you receive `$ARGUMENTS`, do this analysis before proposing any structure:

### Step 1: Identify the domain
What is the topic about? Is it a protocol, a tool, a language, an architecture pattern, a platform? Categorize it.

### Step 2: Find the primary tools or platforms
If the topic involves a specific tool (like Keycloak for IAM, PostgreSQL for databases, Docker for containers), that tool becomes the consistent platform used across all examples. If the topic is tool-agnostic, pick the most common production tool in that space.

### Step 3: Identify prerequisite concepts
What must someone know before they can understand this topic? These become the `01-Foundations/` section.

### Step 4: Decompose into subtopics
Break the topic into 3-6 major subtopic areas. Each becomes its own folder. Think about: core concepts, protocols, data structures, operations, security, integration patterns.

### Step 5: Design the fictional company
Create a fictional company that would realistically use this technology. Give it:
- A name and industry
- A growth trajectory (startup → scale-up → enterprise)
- 5 to 10 evolution phases, each introducing a new business requirement that maps to a technical concept

The company story must be continuous — each phase builds on the previous one.

### Step 6: Identify real-world incidents
For security, reliability, or operational topics, identify 2-5 real CVEs, post-mortems, or famous production failures related to the topic. These will be referenced in the `Security/` or risk section.

## Output Structure

After analysis, propose this folder layout to the user:

```
<topic-slug>-vault/
├── 00-Meta/                    (7 files — same across all topics)
│   ├── Glossary.md
│   ├── Cheat-Sheets.md
│   ├── Learning-Path.md
│   ├── Interview-Prep.md
│   ├── Troubleshooting-Guide.md
│   ├── Architecture-Patterns.md
│   └── Resources.md
├── 01-Foundations/             (3-6 files — prerequisites)
├── 02-<Subtopic-A>/            (3-10 files — with one hub note)
├── 03-<Subtopic-B>/            (3-8 files — with one hub note)
├── 0N-<Subtopic-N>/            (variable)
├── 0X-Security/                (3-5 files — risks, CVEs, mitigations)
├── 0Y-Integration/             (2-5 files — practical implementation)
└── 0Z-Case-Study/              (5-11 files — fictional company evolution)
```

The number of subtopic folders and files depends on the topic's breadth. A narrow topic might have 2 subtopic folders. A broad topic might have 6.

The `00-Meta/` folder is always the same 7 files. Their content adapts to the topic, but the file names and purposes are fixed.

## File Templates

Every file follows one of four templates. Full templates with all sections are in `references/file-templates.md`. Read that file for the complete templates. Here is a summary:

### Hub Note Template
- Frontmatter with `title`, `tags`, `difficulty`, `related`
- Short concept overview (what it is, why it exists, key insight)
- Tables linking to all subtopic files
- Mapping table showing how concepts map to the primary tool/platform
- `## Navigation` section

### Detail Note Template
- Frontmatter
- `## Concept` — what, why, problem solved
- `## <Fictional Company> Scenario` — business need, naive approach failure, solution
- `## <Primary Tool> Implementation` — specific configuration, settings, commands
- `## Technical Details` — protocol behavior, sequence diagrams (Mermaid), data examples
- `## Security Considerations` or `## Risk Considerations` — risks, vulnerabilities, mitigations
- `## Debugging & Troubleshooting` — real production issues, how to diagnose
- `## Best Practices` — industry standards, production recommendations
- `## Navigation` with prerequisites, related concepts, and used-in links

### Case Study Phase Template
- Frontmatter
- `## Business Context` — what the company looks like at this phase
- `## The Challenge` — specific problem they face
- `## Naive Approach (And Why It Fails)` — the obvious-but-wrong solution
- `## The Solution` — what they implement, with tool-specific details
- `## Architecture` — Mermaid diagram
- `## Key Decisions` — trade-offs made and why
- `## What Changes` — what is new compared to the previous phase
- `## Concepts Applied` — list of `[[wiki-links]]` to detail notes
- `## Navigation` with previous phase, next phase, and related concepts

### Meta File Template
- Frontmatter
- Content appropriate to the meta file type (glossary = A-Z definitions; cheat sheets = quick-reference tables; learning path = ordered reading list; interview prep = Q&A format; troubleshooting guide = symptom-based diagnostic paths; architecture patterns = named patterns with problem/solution/diagram; resources = categorized links to RFCs, docs, books, tools)

## Linking Rules

1. **WikiLink format.** Every cross-reference uses `[[filename-without-extension]]`. If you want different display text, use `[[filename|display text]]`. The filename part must match the actual `.md` filename exactly.

2. **Hub notes link out.** A hub note like `Overview.md` links to every file in its folder. It never contains deep explanations — those live in the detail notes.

3. **Bidirectional links.** When you link from A to B, add a backlink from B to A — either in the `## Navigation` section or inline.

4. **No orphan notes.** Every file must have at least 1 prerequisite, 2 related, and 1 downstream link. Verify this before declaring a file complete.

5. **Navigation section.** Every file ends with:
```markdown
## Navigation

**Prerequisites**
- [[...]]

**Related Concepts**
- [[...]]

**Used In**
- [[...]]
```

6. **Wish links are fine.** If a concept is discussed but does not have its own file, a `[[link-to-it]]` that does not resolve is acceptable — Obsidian shows these as unlinked mentions that can become pages later.

## Case Study Rules

The fictional company is the backbone of the vault. It makes abstract concepts concrete.

1. **Give the company a name and industry.** Make it believable. A fintech startup, a logistics company, a game studio, a healthcare platform.

2. **Each phase introduces one new business requirement.** The requirement maps to a technical concept. Phase 1 is always the simplest possible working system. The final phase is the fully-evolved architecture.

3. **Every phase references the detail notes** it applies, using `[[wiki-links]]`.

4. **The naive approach must be realistic.** It should be the solution a junior engineer would reach for — simple, obvious, and wrong in a specific way that the correct solution addresses.

5. **The architecture diagram evolves.** Each phase's Mermaid diagram builds on the previous one, adding new components.

6. **Minimum 5 phases, maximum 12.** Fewer than 5 phases cannot show meaningful evolution. More than 12 becomes repetitive.

## CVE and Post-Mortem Rules

For topics with security, reliability, or operational dimensions:

1. **Find real incidents.** Search the web for CVEs, post-mortem blog posts, or incident reports related to the topic. Use actual CVE numbers and company names.

2. **Explain what happened.** What was the vulnerability or failure? What was the impact?

3. **Explain the fix.** How was it resolved? What architectural or process change prevented recurrence?

4. **Connect to the curriculum.** Which detail note covers the underlying concept? Link to it.

5. **Place in the Security or risk section.** Each incident gets its own callout or subsection within the relevant Security file.

## Quality Gates

Before declaring the vault complete, run these checks with exact commands:

1. **Link audit.** Run:
   ```bash
   grep -roh '\[\[[^]]*\]\]' --include="*.md" . | sed 's/\[\[//g' | sed 's/\]\]//g' | sed 's/|.*//g' | sort -u | while read link; do found=$(find . -iname "${link}.md" | head -1); if [ -z "$found" ]; then echo "BROKEN: [[$link]]"; fi; done
   ```
   Fix every broken link where a file exists but the name doesn't match. Use `sed` for bulk renames:
   ```bash
   find . -name "*.md" -exec sed -i '' 's/\[\[Wrong Name\]\]/[[Correct-Name]]/g' {} \;
   ```

2. **Frontmatter audit.** Every `.md` file must have `title`, `tags`, `difficulty`, and `related`:
   ```bash
   for f in $(find . -name "*.md"); do for field in title tags difficulty; do grep -q "^${field}:" "$f" || echo "MISSING $field: $f"; done; done
   ```

3. **Navigation audit.** Every file ends with `## Navigation`:
   ```bash
   grep -L "^## Navigation" $(find . -name "*.md")
   ```
   This should return nothing. Any file listed is missing its Navigation section.

4. **Orphan audit.** Every file is linked FROM at least one other file:
   ```bash
   for f in $(find . -name "*.md" | sed 's|.*/||' | sed 's/\.md//'); do count=$(grep -rl "\[\[$f\]\]" --include="*.md" . | wc -l); if [ "$count" -eq 0 ]; then echo "ORPHAN: $f"; fi; done
   ```
   Hub notes and the glossary will appear as orphans — that is expected. Everything else must have at least one incoming link.

5. **Case study continuity.** Read all case study phases in order. Verify the company name, tools, and narrative are consistent. Each phase must link to the correct previous and next phases.

6. **Difficulty progression.** Scan difficulty levels:
   ```bash
   grep -rh "^difficulty:" --include="*.md" . | sort | uniq -c
   ```
   Foundations should be mostly beginner. Core topics beginner-to-intermediate. Case study later phases and architecture files should be intermediate-to-advanced.

## Execution Protocol

Follow this exact order. Do not skip phases.

### Phase 0: Analyze and Propose

**Step 0 — Choose output format.** Before any analysis, use AskUserQuestion to ask which output format the user wants:

| Format | What It Produces |
|--------|-----------------|
| **Obsidian vault only** | Multi-file Markdown vault with `[[WikiLinks]]` — best for browsing in Obsidian |
| **PDF only** | Single printable PDF — best for reading linearly or sharing |
| **Both** | Vault + PDF — full flexibility |

If the user chose PDF or Both, note this — Phase 7 will handle PDF generation. All content is written as Markdown files first regardless of format; the PDF is assembled from those files at the end.

**Step 1 — Analyze.** Read `$ARGUMENTS` — this is the topic. Analyze using the steps in "How to Analyze the Topic."

**Step 2 — Propose.** Write and present a **MOC** (Map of Content) as a single Markdown message showing:
- The fictional company name and industry
- The primary tool or platform for examples
- The complete folder structure with every file listed
- The difficulty level for each file
- The case study phase breakdown
- The chosen output format

**Step 3 — Confirm.** Ask the user: "Does this structure look good? Should I add, remove, or change anything?" Wait for approval. Do not write a single file until the user confirms.

### Phase 1: Hub Notes and Foundations
1. Write all hub notes (one per subtopic folder).
2. Write all `01-Foundations/` files.
3. Write the `00-Meta/Glossary.md` (define terms as you create them).

### Phase 2: Core Detail Notes
1. Write all remaining non-hub files in the subtopic folders (`02-*` through `0N-*`).
2. Use parallel agents for independent subtopic folders. Each subtopic folder can be written by a separate agent simultaneously, since files in different folders don't depend on each other.
3. Update hub notes to ensure all links are correct.

### Phase 3: Security, Integration, and Advanced Sections
1. Write the `0X-Security/` files with CVE and post-mortem references.
2. Write the `0Y-Integration/` files with practical implementation guides.
3. Write any remaining advanced-level detail notes.
4. Security and Integration folders are independent — write them in parallel.

### Phase 4: Case Study
1. Write the case study overview.
2. Write all case study phase files in order.
3. Verify the narrative is continuous across all phases.

### Phase 5: Meta Resources
1. Write all remaining `00-Meta/` files: Cheat Sheets, Learning Path, Interview Prep, Troubleshooting Guide, Architecture Patterns, Resources.
2. Update the Glossary with any terms added during Phases 1-4.

### Phase 6: Verification
1. Run the link audit.
2. Run the frontmatter audit.
3. Run the orphan audit.
4. Verify case study continuity.
5. Fix any issues found.
6. Report: total file count, total size, number of Mermaid diagrams, and link resolution stats.

Read `references/file-templates.md` for the complete template for each file type before writing any files. Read `references/example-moc.md` to see a concrete example of a good MOC proposal.

### Phase 7: PDF Generation (only if user chose PDF or Both)

Skip this phase entirely if the user chose Obsidian-only output.

1. **Check for pandoc.** Run `which pandoc`. If not available, attempt `brew install pandoc basictex` (macOS) or `apt-get install pandoc texlive-xetex` (Linux). If installation fails, inform the user and skip PDF generation — the Markdown vault is still complete.

2. **Build a concatenated Markdown file** in reading order inside a `build/` directory. Concatenate all files following the learning path order: foundations first, then core topic sections in numeric order, then security, integration, case study, and finally meta files. Separate each file with `\newpage` so each starts on a new page in the PDF.

3. **Convert wiki links to plain text.** Run sed to strip `[[wiki-links]]` since they are meaningless in a PDF:
   ```bash
   sed -i '' 's/\[\[\([^]|]*\)\]\]/\1/g' build/book.md
   sed -i '' 's/\[\[[^|]*|\([^]]*\)\]\]/\1/g' build/book.md
   ```

4. **Generate the PDF** using pandoc with xelatex engine, table of contents, reasonable margins, and 11pt font:
   ```bash
   pandoc build/book.md --pdf-engine=xelatex --toc --toc-depth=3 \
     --metadata title="<Topic> — Comprehensive Guide" \
     -V geometry:margin=1in -V fontsize=11pt \
     -o build/<topic-slug>-curriculum.pdf
   ```

5. **Report.** State the PDF path, file size, and page count. If Mermaid diagrams are present, note that they render as code blocks in the PDF (pandoc cannot render Mermaid natively — this is a known limitation).

6. **Clean up.** Remove `build/book.md` (the intermediate concatenated file). Keep only the PDF.

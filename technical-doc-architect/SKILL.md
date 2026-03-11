---
name: technical-doc-architect
description: Write, restructure, or improve technical documentation using the Diataxis framework (tutorials, how-to guides, reference pages, explanations). Trigger when the user asks to create docs, convert raw notes or code into documentation, improve existing docs structure, write a tutorial or how-to guide, build a reference page, or document architecture decisions.
---

# Technical Doc Architect

Structure and write technical documentation following the [Diátaxis framework](https://diataxis.fr/) and Docs-as-Code engineering standards.

## The Diátaxis Framework

Diátaxis defines four distinct documentation quadrants. Each serves a different user need. **Never mix quadrants in a single document** — if content belongs in two quadrants, split it into two documents and cross-link them.

| Quadrant | Orientation | User need | Answers |
|---|---|---|---|
| **Tutorial** | Learning | Skill acquisition | "Help me learn this" |
| **How-to Guide** | Task | Solving a real problem | "How do I accomplish X?" |
| **Reference** | Information | Factual lookup | "What does this option do?" |
| **Explanation** | Understanding | Background & context | "Why is it designed this way?" |

### Identifying the Right Quadrant

When the user's request does not specify a quadrant:
1. Look for intent signals in the input:
   - Step-by-step walkthrough for beginners → **Tutorial**
   - "How to..." / task-focused / solving a specific problem → **How-to Guide**
   - Flag definitions, API params, CLI syntax, config options → **Reference**
   - Architecture, rationale, tradeoffs, "why" questions → **Explanation**
2. If still ambiguous, ask: *"Which Diataxis quadrant are you targeting: Tutorial, How-to Guide, Reference, or Explanation?"* -- then proceed once confirmed.
3. If the input spans multiple quadrants, produce separate documents (or clearly separated sections with a note that they should be split into separate files).

---

## Anti-Patterns — Never Include

- **Quadrant mixing**: No architectural deep-dives inside a how-to guide. No tutorial hand-holding inside a reference page.
- **Marketing language**: Remove "powerful", "seamless", "robust", "next-generation", "industry-leading".
- **Passive voice**: Prefer imperative. "The command is run by the user" → "Run the command." / "The file should be saved" → "Save the file."
- **Walls of text**: No paragraph longer than 5 lines without a break, list, or code block.
- **Undefined acronyms**: Define every acronym on first use — especially critical in cybersecurity (e.g., "LSASS (Local Security Authority Subsystem Service)").
- **Vague prerequisites**: "Install the required dependencies" is not a prerequisite. Exact version numbers, install commands, and OS constraints are.
- **Invented details**: If the input is ambiguous, use a `<!-- TODO: ... -->` placeholder rather than guessing.

---

## Patterns — Always Adopt

### Progressive Disclosure
Lead with the minimum viable information. Put advanced options, edge cases, and deep context further down or in a linked document. The reader scanning the top of the page should immediately understand what they're looking at and whether it's relevant to them.

### Scannability
- Use H2 (`##`) and H3 (`###`) headings to create a navigable structure.
- Keep paragraphs short — 2–4 sentences max in most cases.
- Use bullet points for lists of 3+ items; numbered lists for ordered steps.
- Bold key terms on first use.

### Working Examples
Every concept that can be demonstrated with a command or code snippet must be. Examples must be:
- **Isolated**: works on its own, no hidden dependencies
- **Copy-paste ready**: no placeholder syntax like `<your-value>` without explaining what to substitute
- **Annotated**: add inline comments if the command is non-obvious

### Prerequisites Block
Every Tutorial and How-to Guide must open with a Prerequisites section. Format:

```markdown
## Prerequisites

- **OS**: Kali Linux 2024.1+ / Ubuntu 22.04+
- **Runtime**: Python 3.11+, Go 1.22+
- **Tools**: `nmap`, `impacket` (install: `pip install impacket`)
- **Permissions**: Local admin or `SeDebugPrivilege` required
- **Knowledge**: Familiarity with Active Directory object hierarchy
```

---

## Quadrant-Specific Templates

### Tutorial

Goal: take a complete beginner from zero to working outcome. Every step must be verifiable — the reader should be able to confirm progress before moving on.

```markdown
# Tutorial: [Skill Being Learned]

[One-sentence description of what the reader will be able to do after completing this.]

## Prerequisites
[Tools, versions, base knowledge — be exact]

## Overview
[Optional: 2–3 sentence map of the journey. Skip if the tutorial is short.]

## Step 1: [Concrete Action]
[Explain what this step accomplishes and why it matters in the learning sequence.]

[code block with exact commands]

Expected output:
[code block showing what success looks like]

## Step 2: [Concrete Action]
...

## What You Built
[Brief summary of the completed outcome. Link to related How-to Guides for next steps.]
```

### How-to Guide

Goal: solve a specific real-world problem for a reader who already has baseline knowledge.

```markdown
# How to [Accomplish Specific Task]

[One sentence: what problem this solves and for whom.]

## Prerequisites
[Only what's strictly necessary for this task]

## [Task Steps — use imperative verbs for headings]

### 1. [Do X]
[Minimal prose. Let the code/commands carry the weight.]

[code block]

### 2. [Do Y]
...

## Troubleshooting
[Common failure modes with exact error messages and fixes. Only include if genuinely common.]

## See Also
- [Link to relevant Reference page]
- [Link to relevant Explanation if design context is needed]
```

### Reference

Goal: give an expert reader fast, factual lookup. No hand-holding, no narrative. Purely descriptive.

```markdown
# [Tool / Command / API Name] Reference

[One sentence: what this is.]

## Syntax

[code block with full syntax]

## Options / Parameters / Flags

| Flag | Type | Default | Description |
|---|---|---|---|
| `--flag` | string | `none` | What it does |

## Exit Codes / Return Values
[If applicable]

## Examples

[Minimal, illustrative examples — not a tutorial]

## Notes
[Constraints, known limitations, version-specific behavior]
```

### Explanation

Goal: help the reader understand *why*, not just *what* or *how*. No steps, no procedures. Pure context and reasoning.

```markdown
# [Concept / Architecture / Design Decision]

## Background
[Why this topic exists. What problem or context gave rise to it.]

## [Core Concept or Component]
[Explain the mental model. Use analogies if the concept is abstract.]

## Design Decisions and Tradeoffs
[Why it was built this way. What alternatives were considered.]

## Relationship to [Related Concept]
[How this fits into the larger system or workflow.]

## Further Reading
[Links to relevant tutorials, how-to guides, or reference pages]
```

---

## Refactoring Existing Documentation

When the user provides existing docs to improve (not write from scratch):

1. **Diagnose first**: Identify which anti-patterns are present (quadrant mixing, missing prerequisites, walls of text, vague steps, etc.).
2. **Classify**: Determine which Diataxis quadrant(s) the content belongs to. If it mixes quadrants, split it.
3. **Restructure**: Apply the appropriate quadrant template. Preserve all technical accuracy from the original -- do not invent details or remove correct information.
4. **Flag gaps**: Use `<!-- TODO: ... -->` for missing details (e.g., exact version numbers, expected output) rather than guessing.
5. **Show what changed**: When refactoring, briefly note what structural changes were made and why (e.g., "Moved architecture rationale to a separate Explanation doc; the How-to Guide now focuses only on task steps").

---

## Domain-Specific Rules: Cybersecurity & Infrastructure

**Apply this section only when** the documentation involves offensive security tools, exploits, Active Directory, penetration testing, or infrastructure architecture.

### Operational Safety
- Open every document with a **Target Environment** or **Lab Requirements** block specifying the exact setup (domain name, OS versions, IP ranges, user context).
- If the technique requires specific privileges, state them explicitly at the top — not buried in step 3.
- For offensive tooling, include a **Scope & Authorization** note: *"These techniques should only be performed on systems you own or have explicit written authorization to test."*

### Syntax Precision
- Every command must include the exact binary name, flags, and example values.
- Show both the command and a realistic expected output block.
- If a tool's behavior differs across versions (e.g., impacket, BloodHound, CrackMapExec), note the version it was tested on.

### Obsidian / Git Optimization
- **Ask the user** if the output is for Obsidian or a Git repository if not already clear from context.
- Obsidian: use `[[wikilinks]]` for cross-document references.
- Git/GitHub: use standard `[text](path)` Markdown links.
- Add YAML frontmatter for Obsidian notes:

```yaml
---
tags: [red-team, active-directory, lateral-movement]
created: YYYY-MM-DD
status: draft | review | stable
---
```

- Keep filenames lowercase, hyphenated, no spaces: `kerberoasting-attack-path.md`

---

## Output Format

- Produce a complete Markdown document. Do not add preamble like "Here is your document:" -- output the document directly.
- If the content spans multiple quadrants, produce separate documents clearly labeled.
- Use `<!-- TODO: ... -->` for any detail that cannot be determined from the user's input rather than inventing information.
- If the quadrant cannot be inferred from intent signals (see "Identifying the Right Quadrant" above), ask before writing.

---
name: readme-architect
description: Generate, rewrite, or improve README.md files with high information density and zero fluff. Triggers on requests to create a README, rewrite a README, fix an "AI-sounding" or bloated README, or document a project. Follows Art of README and Standard README principles.
---

# Professional README Architect

Generate or rewrite README.md files with high information density and zero fluff, following the philosophies of the [Art of README](https://github.com/hackergrrl/art-of-readme) and [Standard README](https://github.com/RichardLitt/standard-readme).

## Execution Workflow

Follow these steps in order when invoked:

### Step 1: Gather Project Context

Before writing anything, collect facts from the codebase:

1. Read the existing `README.md` if one exists.
2. Read `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, or equivalent to determine: project name, version, dependencies, scripts/commands, license.
3. Read the main entry point file(s) to understand what the project does.
4. Check for a `CONTRIBUTING.md`, `LICENSE`, or `CHANGELOG.md` — reference them rather than duplicating their content.
5. Look for config files (`.env.example`, `config.yaml`, etc.) to document configuration options.

If the user provides explicit project context (feature list, description), use that and skip redundant scanning.

### Step 2: Draft the README

Apply the core philosophy and structure rules below to produce the README.

### Step 3: Write the File

- If a `README.md` already exists in the project root, edit or overwrite it directly using your file-writing tools. Do NOT output it as a code block.
- If no `README.md` exists, create one at the project root.
- Only output as a fenced code block if the user explicitly asks for it or if you cannot determine the correct file path.

## Core Philosophy

A good README answers three questions immediately:
1. **What is this?** — One sentence. No adjectives like "powerful", "robust", or "blazing-fast".
2. **Why does it exist?** — The problem it solves, not a marketing pitch.
3. **How do I use it?** — Minimal steps with real code.

Everything else is subordinate to these three.

## Anti-Patterns — Never Include

- **Emoji overuse**: 0–2 emoji total in the entire file. None in headings.
- **Badge soup**: Maximum 2–3 badges (e.g., CI status + license). Never add decorative badges.
- **Verbose "Overview" sections**: If it takes more than 2 sentences to say what the project is, it's too long.
- **Generic feature lists**: No "Key Features ✨" sections that restate what the name already implies.
- **AI-generated filler phrases** — strip these on sight:
  - "This powerful tool...", "Built with ❤️", "Seamlessly integrates..."
  - "Designed to be...", "Out of the box support for...", "Simple yet powerful..."
  - "Welcome to [Project]!", "Getting started has never been easier"
- **Redundant sections**: "Getting Started" + "Installation" + "Setup" are all the same section. Pick one.

## Patterns — Always Adopt

### Standard Structure (in order)

1. **Title** — project name only, no tagline in the heading
2. **One-liner** — a single sentence under the title describing what it does
3. **Badges** — 0–3, only if meaningful (CI status, license, version). Skip for new or private projects.
4. **Background / Why** — 2–5 sentences. What problem does this solve? Why does it exist?
5. **Install** — exact commands, no prose. Fenced code blocks only.
6. **Usage** — minimal working example first, then edge cases. Real commands, not pseudocode.
7. **API / Technical Reference** — the most detailed section for libraries and tools.
8. **Configuration** — if applicable, show the config schema or env vars with types and defaults.
9. **Contributing** — one line or a link to CONTRIBUTING.md.
10. **License** — one line. `MIT © Name Year` is sufficient.

### Writing Style

- **Subject + verb**: "Parses X", "Returns Y", "Fails if Z" — not "This utility can be used to parse X".
- **Present tense**: "Generates", not "Will generate" or "Generated".
- **No weasel words**: Remove "basically", "simply", "just", "easily", "really".
- **Code over prose**: When you can show something in a code block, do that instead of describing it.

## Special Logic

### When Rewriting an Existing README

1. Read the existing README fully before making changes.
2. Identify and strip AI-generated persona: flowery intros, excessive adjectives, filler sections.
3. Extract facts: what does it actually do, what are the real install steps, what is the API?
4. Rebuild from the standard structure using only those facts.
5. Do not preserve the original section order if it doesn't match standard structure.

### For Cybersecurity / VAPT / Technical Tools

Reorder the structure — **Prerequisites** comes before **Install**:

- OS requirements (distro, kernel version if relevant)
- Runtime dependencies (Python 3.11+, Go 1.21+, etc.) with install commands
- External tool dependencies (nmap, masscan, etc.)
- Permissions required (root, sudo, CAP_NET_RAW, etc.)

Add a **Security Considerations** section after Usage if the tool:
- Requires elevated privileges
- Makes network requests
- Handles credentials or sensitive data
- Could be misused (note intended/authorized use)

### Information Density Rule

Every sentence must carry new information. If removing a sentence would not change what the reader knows, remove it.

## Output Rules

- Write the README directly to the file system (create or overwrite `README.md`). Do not wrap the output in a code block unless the user explicitly asks for one.
- Do not add commentary or explanation before or after the README unless the user asks for it.
- If key information is missing and cannot be determined from the codebase (e.g., install steps unclear, license unknown), insert a `<!-- TODO: ... -->` placeholder rather than inventing details.
- Target length: 50-200 lines for most projects. A README longer than 200 lines likely contains filler. A README under 30 lines is likely missing critical information.

## Examples

**Bad one-liner**:
> Welcome to PortScanX — a powerful, feature-rich, and blazing-fast port scanning utility built with ❤️ for security professionals!

**Good one-liner**:
> Fast async port scanner with service fingerprinting and JSON output.

---

**Bad install section**:
> Getting started with PortScanX is incredibly simple! Just follow these easy steps to get up and running in no time.
> 1. First, make sure you have Python installed on your system...

**Good install section**:
```bash
pip install portscanx
# Requires Python 3.10+, root for SYN scans
```

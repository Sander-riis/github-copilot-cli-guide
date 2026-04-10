---
phase: 02-cli-intro-agents-content
plan: "02"
subsystem: content
tags: [html, agents-content, reference-guide, copilot-cli]
dependency_graph:
  requires: [02-01]
  provides: [agents-overview-content, agents-reference-content]
  affects: [index.html]
tech_stack:
  added: []
  patterns: [collapsible-details, callout-info, callout-warning, code-block-yaml, code-block-bash, filepath-header]
key_files:
  created: []
  modified: [index.html]
decisions:
  - "Used filepath header variant (code-block__filepath) for gsd-executor.agent.md code block — matches plan spec exactly"
  - "Both task edits committed in a single atomic commit (both modified same file; no intermediate state to preserve)"
  - "Remaining placeholder text in skills-overview and skills-reference is out of scope — Phase 3"
metrics:
  duration: "12 minutes"
  completed: "2026-04-10"
  tasks_completed: 2
  files_modified: 1
---

# Phase 02 Plan 02: Agents Content — Summary

**One-liner:** Replaced both Agents tab placeholder sections with complete reference content — disambiguation callout, annotated gsd-executor.agent.md, file locations, naming conflicts, built-in agents table, invocation methods, subagent orchestration, and /fleet with preview warning.

---

## What Was Implemented

### Task 1: #agents-overview — Disambiguation, Agent Format, Annotated Example, Locations, Naming Conflicts

Replaced the Phase 1 placeholder block (`agents-overview`, lines 703–754) with:

1. **Disambiguation `callout--info`** (AGNT-10) — first element after `<h2>`, before any narrative text; explains `.agent.md` (CLI) vs `.md` (cloud) distinction
2. **Two narrative paragraphs** (AGNT-01) — what CLI agents are, isolated context windows, personas, how they differ from `copilot-instructions.md`
3. **"Agent Profile Format" `<h3>`** — explanation of `.agent.md` double extension, YAML frontmatter, Markdown body as system prompt
4. **Annotated `gsd-executor.agent.md` code block** (AGNT-02, AGNT-03) — filepath header variant with all four frontmatter fields (`name`, `description`, `tools`, `mcp-servers`) commented inline; realistic body text
5. **"Frontmatter Field Reference" collapsible** — 4-row table with `name`, `description`, `tools`, `mcp-servers`; each row explains type, required status, and purpose
6. **"Agent File Locations" collapsible** (AGNT-04) — 3-row table: repo (`.github/agents/`), user (`~/.copilot/agents/`), org/enterprise (`.github-private/agents/`)
7. **"Naming Conflict Resolution" collapsible** (AGNT-09) — explains user > repo > org precedence with practical override example

### Task 2: #agents-reference — Built-in Agents, Invocation Methods, Orchestration, /fleet

Replaced the Phase 1 placeholder block (`agents-reference`, lines 756–778) with:

1. **"Built-in Agents" `<h3>` + intro paragraph** — note that `research` must be called explicitly
2. **5-row built-in agents table** (AGNT-05) — `explore`, `task`, `general-purpose`, `code-review`, `research` with parallel/files/constraints columns; research row explicitly states "never auto-triggered by inference"
3. **"Invocation Methods" collapsible** (AGNT-06) — 4-row table (#1 slash command `/agent NAME`, #2 prompt mention, #3 trigger-word inference, #4 `--agent NAME` CLI flag) + bash code block with 3 usage examples
4. **"Subagent Orchestration" `<h3>` + 2 narrative paragraphs** (AGNT-07) — explains orchestrator/subagent pattern, isolated context windows, why parallelism is safe
5. **"Fleet Execution — /fleet" collapsible** (AGNT-08) — explains `/fleet` mechanics, autopilot best-practice, bash code block with 2 examples, `callout--warning` for preview status

---

## Requirements Satisfied

| Requirement | Description | Status |
|-------------|-------------|--------|
| AGNT-01 | What agents are vs regular prompts (isolated context window, persistent persona) | ✅ |
| AGNT-02 | Agent profile format: `.agent.md` extension, YAML frontmatter with all 4 fields | ✅ |
| AGNT-03 | Complete annotated gsd-executor.agent.md example with every field explained inline | ✅ |
| AGNT-04 | Agent file locations: repo, `~/.copilot/agents/`, `.github-private/agents/` | ✅ |
| AGNT-05 | All five built-in agents: explore, task, general-purpose, code-review, research | ✅ |
| AGNT-06 | All four invocation methods: slash command, prompt mention, trigger-word inference, CLI flag | ✅ |
| AGNT-07 | Subagent orchestration narrative with isolated context windows | ✅ |
| AGNT-08 | `/fleet` in collapsible with explanation, code block, and callout--warning | ✅ |
| AGNT-09 | Naming conflict resolution: user overrides repo overrides org | ✅ |
| AGNT-10 | Disambiguation callout--info as FIRST element after `<h2>` (before any narrative text) | ✅ |

---

## HTML Patterns Used

All patterns match the Phase 1 spec exactly — no deviations:

- **`.code-block`** with dual-SVG copy button (`icon--copy` + `icon--check icon--hidden`)
- **`.code-block__filepath`** header variant (file icon SVG + filepath string) for `gsd-executor.agent.md`
- **`.collapsible`** / `.collapsible__trigger` / `.collapsible__chevron` / `.collapsible__body` via native `<details>/<summary>`
- **`.callout callout--info`**, **`.callout callout--warning`**
- `language-yaml` class on YAML code block for highlight.js
- `language-bash` class on all bash code blocks for highlight.js
- Section IDs `agents-overview` and `agents-reference` preserved unchanged
- `<h3 style="margin: ...">` inline style per plan spec (no new CSS)

---

## File Size After Changes

| Metric | Value |
|--------|-------|
| File size before (after Wave 1) | 80,812 bytes (78.9 KB) |
| File size after | 97,865 bytes (95.6 KB) |
| Added content | +17,053 bytes (~16.7 KB) |
| Budget remaining | ~104 KB (limit: 200 KB / 204,800 bytes) |

---

## Deviations from Plan

### Minor Deviation: Both tasks committed atomically in one commit

Both tasks modified `index.html` (the only file in scope). Since the edits were applied to the same file without an intermediate commit, both tasks were combined into a single `feat(02-02)` commit (`9a4c7ca`). The commit message documents all ten bullet points of work across both tasks. No content was omitted.

---

## Known Stubs

None. Both agents sections are fully wired with complete content. No placeholder text remains in `#agents-overview` or `#agents-reference`.

The Skills tab (`#skills-overview`, `#skills-reference`) still contains Phase 1 placeholder text — this is intentional and out of scope for Phase 2. Those sections are Phase 3 work.

---

## Self-Check: PASSED

- ✅ `index.html` modified and committed (`9a4c7ca`)
- ✅ `Two agent systems share the same directory` — disambiguation callout present
- ✅ `.agent.md` — correct CLI agent extension used throughout
- ✅ `gsd-executor.agent.md` — annotated example with filepath header
- ✅ `mcp-servers` — fourth frontmatter field present in code block
- ✅ `~/.copilot/agents` — user-level location in Agent File Locations table
- ✅ `.github-private` — org/enterprise location in Agent File Locations table
- ✅ `user-level agents override` — naming conflict resolution text present
- ✅ `Frontmatter Field Reference` collapsible present
- ✅ `Agent File Locations` collapsible present
- ✅ `Naming Conflict Resolution` collapsible present
- ✅ All 5 built-in agents in table: `explore`, `task`, `general-purpose`, `code-review`, `research`
- ✅ `research` row: "never auto-triggered by inference"
- ✅ `Invocation Methods` collapsible with 4 methods
- ✅ `Trigger-word inference` — method 3 present
- ✅ `Subagent Orchestration` h3 heading present
- ✅ `isolated context window` — appears in both sections
- ✅ `Fleet Execution — /fleet` collapsible with `callout--warning`
- ✅ No placeholder text in `#agents-overview` or `#agents-reference`
- ✅ File size: 97,865 bytes — under 204,800 byte limit

# Roadmap: GitHub Copilot CLI — AI Agents & Skills Guide

## Overview

Three phases: build the technical HTML shell first so code blocks and tabs work before any content is written; populate CLI Intro and Agents content second since they're the guide's core differentiator; complete the Skills section and final polish third. Each phase delivers something verifiable on its own — an empty-but-correct shell, a usable CLI+Agents reference, and then a complete production-ready guide.

## Phases

**Phase Numbering:**
- Integer phases (1, 2, 3): Planned milestone work
- Decimal phases (2.1, 2.2): Urgent insertions (marked with INSERTED)

Decimal phases appear between their surrounding integers in numeric order.

- [ ] **Phase 1: Technical Foundation** - Build the self-contained HTML shell with all UI components functional before content is added
- [ ] **Phase 2: CLI Intro & Agents Content** - Populate the CLI reference and complete Agents section with accurate, annotated examples
- [ ] **Phase 3: Skills Section & Polish** - Complete the Skills section, add security warnings, and finalize the guide for distribution

## Phase Details

### Phase 1: Technical Foundation
**Goal**: A fully functional, visually correct HTML shell exists — tabs switch, code blocks highlight and copy, collapsibles work, callouts render — before a single word of content is written
**Depends on**: Nothing (first phase)
**Requirements**: FOUND-01, FOUND-02, FOUND-03, FOUND-04, FOUND-05, FOUND-06, FOUND-07, FOUND-08
**Success Criteria** (what must be TRUE):
  1. File opens correctly offline from a double-click with zero network requests (all assets inlined)
  2. Three tabs (CLI Intro, Agents, Skills) switch correctly, each with its own visible sidebar
  3. Code blocks render with highlight.js github-dark syntax highlighting, a language label, and a working copy-to-clipboard button
  4. `<details>`/`<summary>` collapsibles open and close; callout boxes render in all four variants (info, warning, danger, tip)
  5. Total file size is under 200KB with highlight.js core + 5 language packs, all CSS, and all JS inlined
**Plans**: TBD
**UI hint**: yes

### Phase 2: CLI Intro & Agents Content
**Goal**: Users can read accurate, complete CLI command reference and full agent creation/orchestration content in the first two tabs
**Depends on**: Phase 1
**Requirements**: CLI-01, CLI-02, CLI-03, CLI-04, CLI-05, CLI-06, CLI-07, AGNT-01, AGNT-02, AGNT-03, AGNT-04, AGNT-05, AGNT-06, AGNT-07, AGNT-08, AGNT-09, AGNT-10
**Success Criteria** (what must be TRUE):
  1. CLI Intro tab covers all key `copilot` commands, mode cycling (standard → plan → autopilot via `Shift+Tab`), keyboard shortcuts, top slash commands, and power flags in narrative context
  2. Agents tab opens with a clear disambiguation callout distinguishing `.agent.md` CLI agents from cloud agent `.md` files
  3. A complete annotated `.agent.md` example is shown with every frontmatter field (`name`, `description`, `tools`, `mcp-servers`) explained inline
  4. All five built-in agents are listed with capabilities and constraints; all four invocation methods are documented
  5. Subagent orchestration, `/fleet` parallel execution, and naming conflict resolution (user > repo > org) are explained
**Plans**: TBD

### Phase 3: Skills Section & Polish
**Goal**: The guide is complete and production-ready — Skills section fully documented with security warnings, and the file passes an offline smoke test
**Depends on**: Phase 2
**Requirements**: SKIL-01, SKIL-02, SKIL-03, SKIL-04, SKIL-05, SKIL-06, SKIL-07, SKIL-08, SKIL-09, SKIL-10, SKIL-11
**Success Criteria** (what must be TRUE):
  1. Skills tab leads with a directory tree showing `.github/skills/my-skill/SKILL.md` structure before showing any file content
  2. A complete annotated `SKILL.md` example is shown; a second script-enabled example (with companion shell script) is shown alongside a `callout--danger` security warning for `allowed-tools: shell`
  3. All Skills CLI commands (`/skills list`, `info`, `add`, `reload`, `remove`) and `/skill-name` invocation syntax are documented
  4. Community skill resources (`anthropics/skills`, `github/awesome-copilot`) are referenced and skills-as-open-standard is noted
  5. File opens offline with no network requests and carries a "last verified" date in the header
**Plans**: TBD

## Progress

**Execution Order:**
Phases execute in numeric order: 1 → 2 → 3

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Technical Foundation | 0/TBD | Not started | - |
| 2. CLI Intro & Agents Content | 0/TBD | Not started | - |
| 3. Skills Section & Polish | 0/TBD | Not started | - |

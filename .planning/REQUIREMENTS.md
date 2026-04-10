# Requirements: GitHub Copilot CLI — AI Agents & Skills Guide

**Defined:** 2026-04-10
**Core Value:** Experienced Copilot CLI users can understand, create, and orchestrate agents and skills without hunting through scattered documentation.

---

## v1 Requirements

### Foundation

- [ ] **FOUND-01**: Page is delivered as a single self-contained `.html` file with no external dependencies
- [ ] **FOUND-02**: All CSS, JavaScript, and syntax highlighting assets are inlined into the file
- [ ] **FOUND-03**: Page uses a dark terminal-inspired theme based on the GitHub Primer dark palette (`#0d1117` background, `#c9d1d9` text)
- [ ] **FOUND-04**: Page is structured into three main tabs: CLI Intro, Agents, Skills
- [ ] **FOUND-05**: Each tab section supports collapsible subsections via `<details>`/`<summary>`
- [ ] **FOUND-06**: Code blocks use highlight.js 11.11.1 with the `github-dark` theme for syntax highlighting
- [ ] **FOUND-07**: Every code block has a copy-to-clipboard button
- [ ] **FOUND-08**: File size stays under 200KB uncompressed

### CLI Intro Section

- [x] **CLI-01**: Section opens with a narrative explanation of how the Copilot CLI works (not a beginner walkthrough — context for power users)
- [x] **CLI-02**: Key command-line patterns are shown: `copilot`, `copilot -p "..."`, `copilot -sp "..."`, `copilot --agent NAME`
- [x] **CLI-03**: Mode cycling is explained: standard → plan → autopilot via `Shift+Tab`
- [x] **CLI-04**: Key shortcuts are documented: `@FILENAME` context inclusion, `!COMMAND` shell passthrough, `Esc`, `Ctrl+L`, `/help`
- [x] **CLI-05**: Top slash commands are covered narratively (at minimum: `/plan`, `/fleet`, `/model`, `/review`, `/delegate`, `/research`, `/diff`, `/pr`, `/skills`, `/agent`)
- [x] **CLI-06**: Power-user flags are listed: `--allow-all-tools`, `--autopilot`, `--agent`, `--resume`, `--allow-tool`, `--deny-tool`
- [x] **CLI-07**: The `/research` agent's special status is noted — only invokable via `/research`, never auto-triggered

### Agents Section

- [x] **AGNT-01**: Section explains what agents are and how they differ from regular Copilot prompts (separate context window, persistent persona)
- [x] **AGNT-02**: Agent profile file format is shown: `.agent.md` extension (not `.md`), YAML frontmatter with `name`, `description`, `tools`, `mcp-servers` fields
- [x] **AGNT-03**: A complete annotated `.agent.md` example file is included — every field explained inline
- [x] **AGNT-04**: Agent file locations are documented: `.github/agents/` (repo), `~/.copilot/agents/` (user), `/agents/` in `.github-private` (org/enterprise)
- [x] **AGNT-05**: All five built-in agents are listed with their capabilities and constraints: `explore`, `task`, `general-purpose`, `code-review`, `research`
- [x] **AGNT-06**: All four agent invocation methods are covered: `/agent NAME`, prompt mention, trigger-word inference, `--agent NAME` flag
- [x] **AGNT-07**: Subagent orchestration is explained: main agent as orchestrator, subagents with isolated context windows, why this matters
- [x] **AGNT-08**: `/fleet` command is explained: how main agent decomposes tasks and runs subagents in parallel
- [x] **AGNT-09**: Naming conflict resolution is documented: user-level overrides repo-level, repo-level overrides org-level
- [x] **AGNT-10**: A disambiguation callout is included early in the section distinguishing CLI agents (`.agent.md`) from cloud agents (GitHub.com UI)

### Skills Section

- [ ] **SKIL-01**: Section explains what skills are and how they differ from agents and custom instructions
- [ ] **SKIL-02**: Skills as an open standard (Agent Skills spec, works across Copilot CLI, cloud agent, VS Code agent mode) is noted
- [ ] **SKIL-03**: Skill directory structure is shown: `.github/skills/my-skill/SKILL.md` (subdirectory + exact filename `SKILL.md`)
- [ ] **SKIL-04**: SKILL.md frontmatter fields are documented: `name` (required, lowercase-hyphens), `description` (required), `license` (optional), `allowed-tools` (optional)
- [ ] **SKIL-05**: A complete annotated `SKILL.md` example is included — every field and body section explained
- [ ] **SKIL-06**: A script-enabled skill example is shown: `SKILL.md` + companion shell script, demonstrating `allowed-tools: shell`
- [ ] **SKIL-07**: A security warning is included alongside any `allowed-tools: shell` or `allowed-tools: bash` usage
- [ ] **SKIL-08**: Skill file locations are documented: `.github/skills/` (project), `~/.copilot/skills/` (personal)
- [ ] **SKIL-09**: All key Skills CLI commands are shown: `/skills list`, `/skills info SKILL`, `/skills add PATH`, `/skills reload`, `/skills remove SKILL-DIRECTORY`
- [ ] **SKIL-10**: Skill invocation syntax is shown: `/skill-name` in a prompt
- [ ] **SKIL-11**: Community skill resources are referenced: `anthropics/skills`, `github/awesome-copilot`

---

## v2 Requirements

### Advanced Content

- **ADV-01**: Custom model provider (BYOK) env vars: `COPILOT_PROVIDER_BASE_URL`, `COPILOT_PROVIDER_TYPE`, `COPILOT_PROVIDER_API_KEY`, `COPILOT_MODEL`
- **ADV-02**: Session management deep dive: `/session`, `--resume`, `--continue`, `/share` workflows
- **ADV-03**: Fleet + autopilot walkthrough with `@AGENT-NAME` syntax for directing subtasks to custom agents
- **ADV-04**: Tool permission patterns deep dive: `/yolo`, per-tool allow/deny, SKILL.md `allowed-tools` patterns

### Polish

- **POL-01**: Hash-based tab state restoration (`?tab=agents` preserves direct links)
- **POL-02**: Mobile-responsive layout
- **POL-03**: Keyboard navigation between tabs

---

## Out of Scope

| Feature | Reason |
|---------|--------|
| Installation / authentication walkthrough | Assumes existing Copilot CLI users; beginner content |
| GitHub Copilot Chat / IDE features | Explicitly out of scope — CLI only |
| Cloud agent management (GitHub.com UI) | Different product surface; brief disambiguation note only |
| MCP server creation | Deep separate topic; mention `/mcp add` at most |
| Plugin API creation | Wrong layer for this audience |
| Billing / quota details | Administrative, not developer workflow |
| Old `gh copilot suggest/explain` commands | Different product; could confuse readers |
| Multi-page site or external hosting | Single self-contained file only |

---

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| FOUND-01 | Phase 1 | Pending |
| FOUND-02 | Phase 1 | Pending |
| FOUND-03 | Phase 1 | Pending |
| FOUND-04 | Phase 1 | Pending |
| FOUND-05 | Phase 1 | Pending |
| FOUND-06 | Phase 1 | Pending |
| FOUND-07 | Phase 1 | Pending |
| FOUND-08 | Phase 1 | Pending |
| CLI-01 | Phase 2 | Complete |
| CLI-02 | Phase 2 | Complete |
| CLI-03 | Phase 2 | Complete |
| CLI-04 | Phase 2 | Complete |
| CLI-05 | Phase 2 | Complete |
| CLI-06 | Phase 2 | Complete |
| CLI-07 | Phase 2 | Complete |
| AGNT-01 | Phase 2 | Complete |
| AGNT-02 | Phase 2 | Complete |
| AGNT-03 | Phase 2 | Complete |
| AGNT-04 | Phase 2 | Complete |
| AGNT-05 | Phase 2 | Complete |
| AGNT-06 | Phase 2 | Complete |
| AGNT-07 | Phase 2 | Complete |
| AGNT-08 | Phase 2 | Complete |
| AGNT-09 | Phase 2 | Complete |
| AGNT-10 | Phase 2 | Complete |
| SKIL-01 | Phase 3 | Pending |
| SKIL-02 | Phase 3 | Pending |
| SKIL-03 | Phase 3 | Pending |
| SKIL-04 | Phase 3 | Pending |
| SKIL-05 | Phase 3 | Pending |
| SKIL-06 | Phase 3 | Pending |
| SKIL-07 | Phase 3 | Pending |
| SKIL-08 | Phase 3 | Pending |
| SKIL-09 | Phase 3 | Pending |
| SKIL-10 | Phase 3 | Pending |
| SKIL-11 | Phase 3 | Pending |

**Coverage:**
- v1 requirements: 36 total
- Mapped to phases: 36
- Unmapped: 0 ✓

---
*Requirements defined: 2026-04-10*
*Last updated: 2026-04-10 after initial research synthesis*

# Phase 2 Context: CLI Intro & Agents Content

**Phase:** 02 — CLI Intro & Agents Content
**Created:** 2026-04-10
**Status:** Ready for research and planning

---

## Domain

Populate the CLI Intro and Agents tabs with accurate, complete content inside the Phase 1 HTML shell. No structural changes to the page — only the content inside existing `<section>` elements changes.

---

## Decisions

### Content Style
- **Narrative walkthroughs** — every major topic gets a `<section>` with a short explanatory paragraph followed by code blocks or annotated examples
- Not a quick-reference card page; readers should be able to *read* through each section and understand the topic
- Tone: technical but accessible — assumes existing Copilot CLI familiarity, not a beginner

### Section Structure
- **2 sections per tab** — matching the Phase 1 placeholder structure exactly
  - CLI Intro tab: `cli-overview` (commands, mode cycling, shortcuts, power flags) + `cli-reference` (slash commands)
  - Agents tab: `agents-overview` (what agents are, disambiguation, file format) + `agents-reference` (built-ins, invocation, orchestration, fleet)
- Sidebar anchors already exist; content must flow to fit within these 2 sections per tab
- Pack related topics together using `<details>`/`<summary>` collapsibles within each section to avoid visual overload

### .agent.md Example
- **Use a GSD-style agent** (e.g., `gsd-executor` or similar) as the base for the annotated example
- The example should be a real-looking agent with actual frontmatter (`name`, `description`, `tools`, `mcp-servers`) and a meaningful body — not a toy example
- Annotate every field inline using HTML comments or a split-panel layout
- The example must satisfy AGNT-02 and AGNT-03

### Code Examples
- **Real syntax** — use actual copilot CLI commands exactly as they work today
  - `copilot` (bare), `copilot -p "..."`, `copilot -sp "..."`, `copilot --agent NAME`
  - `/fleet`, `/research`, `/plan`, `/model`, `/review`, `/delegate`, `/diff`, `/pr`, `/skills`, `/agent`
  - `@FILENAME`, `!COMMAND`, `Esc`, `Ctrl+L`, `/help`
- No disclaimer notes or "approximate" labels — accuracy is the goal
- All code blocks tagged with correct language (`bash`, `yaml`, `markdown`) for highlight.js

### Carrying Forward from Phase 1
- Component classes to use: `.code-block`, `.collapsible`, `.callout--info`, `.callout--warning`, `.callout--danger`, `.callout--tip`
- All CSS variables already defined; no new CSS needed unless content genuinely requires it
- No CDN links — everything remains self-contained
- File size budget: ~135KB remaining (current: 69.2KB, limit: 200KB)

---

## Requirements Covered

This phase must satisfy:
- CLI-01, CLI-02, CLI-03, CLI-04, CLI-05, CLI-06, CLI-07
- AGNT-01, AGNT-02, AGNT-03, AGNT-04, AGNT-05, AGNT-06, AGNT-07, AGNT-08, AGNT-09, AGNT-10

---

## Canonical Refs

- `.planning/REQUIREMENTS.md` — full requirement text for CLI-01–07, AGNT-01–10
- `.planning/ROADMAP.md` — Phase 2 success criteria
- `.planning/phases/01-technical-foundation/01-UI-SPEC.md` — component classes, CSS tokens, layout specs
- `.planning/research/ARCHITECTURE.md` — HTML structure patterns, component usage
- `index.html` — the live Phase 1 shell; content goes into existing `<section>` elements

---

## Out of Scope (Phase 2)

- Skills tab content — Phase 3
- Hash-based tab state restoration (POL-01) — Phase 3
- Mobile-responsive layout (POL-02) — Phase 3
- Any new CSS or JS — Phase 1 is complete; Phase 2 is content only
- v2 requirements (ADV-01–04) — future milestone

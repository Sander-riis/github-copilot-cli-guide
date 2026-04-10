# Project Research Summary

**Project:** GitHub Copilot CLI — AI Agents & Skills Guide
**Domain:** Single-file interactive HTML developer reference guide
**Researched:** 2025-01-10
**Confidence:** HIGH

---

## Executive Summary

This is a single self-contained `.html` file that serves as a comprehensive reference guide for experienced developers using GitHub Copilot CLI — covering the `copilot` command, custom agents (`.agent.md` files), and agent skills (`SKILL.md` directories). The right approach is zero-dependency: all CSS and JS inlined, system fonts only, vanilla JS (~50 lines total) for tabs and copy buttons, native `<details>`/`<summary>` for collapsibles, and highlight.js (core + 5 languages, ~33KB) inlined for syntax highlighting. The file should open offline from a double-click and share cleanly as an email attachment — no build step required, no CDN dependencies. Total target size is 75–110KB.

The content is structured across three conceptually distinct tabs: CLI Intro → Agents → Skills. The recommended architecture is a CSS Grid layout (fixed header + scrollable sidebar + scrollable main content), with three article panels swapped by the tab navigation and a scrollspy sidebar that highlights the active section. Code blocks are the most important component — every one needs a copy button, language label, and optionally a filepath label. Content follows a strict pattern: one sentence of context → minimal code example → explanation → real-world annotated example → collapsed reference details.

The highest-risk area is content accuracy. GitHub Copilot CLI is an actively evolving product with two common confusion traps: (1) conflating the legacy `gh copilot suggest/explain` commands with the modern standalone `copilot` command, and (2) confusing Copilot CLI custom agents (`.agent.md` files, invoked locally) with Copilot cloud agents (`.md` files, invoked from GitHub Issues). Every command example must be verified against the official CLI command reference. The skill structure is also non-obvious — each skill is a directory containing a file named exactly `SKILL.md`, not a flat `.md` file.

---

## Key Findings

### Recommended Stack

The stack is deliberately minimal in service of the single-file constraint. highlight.js 11.11.1 (core-only build + 5 language packs: bash, yaml, json, markdown, shell) with the `github-dark` theme is the right syntax highlighter — it uses GitHub's own Primer color palette, is exactly on-brand for a Copilot guide, and costs only ~33KB inline. Vanilla JS handles all interactivity. CSS Custom Properties provide a full theming system without a preprocessor. System font stacks eliminate all external network requests.

> **Note:** ARCHITECTURE.md suggests Prism.js for the syntax highlighter while STACK.md recommends highlight.js. Both are valid; the recommendation is **highlight.js** because its `github-dark` theme is directly sourced from GitHub's production CSS, making it the more on-brand choice for this specific guide.

**Core technologies:**
- **highlight.js 11.11.1** (core + bash/yaml/json/markdown/shell): Syntax highlighting — GitHub-dark theme is on-brand; 33KB inline vs 125KB for full bundle
- **CSS Custom Properties**: Theming system — no preprocessor, runtime-configurable, GitHub Primer dark palette tokens
- **Vanilla JS (~50 lines)**: Tab switching + copy-to-clipboard — no library needed; `navigator.clipboard.writeText()` is baseline-supported
- **`<details>`/`<summary>` (native HTML)**: Collapsibles — zero JS, accessible by default, CSS-animatable
- **System font stack**: Typography — no web font requests, no FOUT, developer audience already has quality monospace fonts
- **CSS Grid**: Layout — header + sidebar + main in ~20 lines of CSS, no framework

**Final size budget:** ~75–110KB total (all content, CSS, JS, syntax highlighting) — lighter than most framework apps.

**What NOT to use:** React/Vue/Svelte, jQuery, Bootstrap/Tailwind, Webpack/Vite, CDN links in final HTML, Google Fonts, Font Awesome, highlight.js full bundle, localStorage for state.

See [STACK.md](.planning/research/STACK.md) for full details, color palette, and assembly strategy.

---

### Expected Features

Content users expect from a "Copilot CLI agents & skills guide." All verified against official GitHub Docs (docs.github.com, 2025).

**Must have (table stakes):**
- Core CLI commands cheat sheet (`copilot`, `-p`, `-s`, flags)
- Interactive slash commands reference (curated top 8–10)
- Keyboard shortcut table (`Shift+Tab` mode cycling, `@FILENAME`, `!CMD`)
- Full annotated agent profile example (`.agent.md` format with every frontmatter field explained)
- Agent file locations table (`.github/agents/`, `~/.copilot/agents/`, org/enterprise)
- Built-in agents inventory (all 5: `explore`, `task`, `general-purpose`, `code-review`, `research`)
- Full annotated `SKILL.md` example with directory structure
- Skill file locations (`.github/skills/`, `~/.copilot/skills/`)
- Skills CLI commands (`/skills list`, `info`, `add`, `reload`, `remove`)
- Subagent/orchestration explanation (separate context window)
- `/fleet` command explanation (parallel orchestration)
- Agent invocation methods (all 4: `/agent`, mention, inference, `--agent` flag)
- Skills vs agents vs custom instructions distinction

**Should have (differentiators that elevate to "excellent"):**
- Mode cycling visual (standard → plan → autopilot, `Shift+Tab`)
- Fleet + autopilot workflow walkthrough (the power pattern)
- Script-enabled skill example (`SKILL.md` + companion shell script)
- Built-in agents cheat sheet (side-by-side constraints)
- Naming conflict resolution rules (user > repo > org)
- Tool permission patterns (`--allow-all-tools`, `--allow-tool`, `--deny-tool`)
- `@AGENT-NAME` syntax inside `/fleet` prompts
- Community skills resources (`anthropics/skills`, `github/awesome-copilot`)

**Defer (v2+ or collapse as optional):**
- Custom model provider (BYOK env vars) — add as collapsed reference section
- Session management deep dive (`/session`, `--resume`, `/share`)
- Plugin system — out of scope; one-line reference only

**Hard anti-features (do NOT include):**
- Installation/setup walkthrough, authentication flow, IDE/Chat features, MCP server creation, plugin API creation, billing details, org policy configuration

See [FEATURES.md](.planning/research/FEATURES.md) for accurate technical details (verified frontmatter fields, built-in agent table, modes reference).

---

### Architecture Approach

The architecture is a classic docs-site layout adapted for a single HTML file: a fixed header containing the tab nav, a sticky scrollable sidebar with per-tab navigation lists, and a main content area with three article panels (only one visible at a time). CSS Grid provides the shell (`header / sidebar / main`) in ~20 lines. JavaScript components are named functions initialized from a single `DOMContentLoaded` handler. The ARCHITECTURE.md provides a complete 8-stage build order that should be followed precisely — it correctly sequences layout validation before component development before content population.

**Major components:**
1. **Tab Navigation** — `role="tablist"` buttons that show/hide article panels and corresponding sidebar navs; ~30 lines of JS with `aria-selected` for accessibility
2. **Code Block Component** — the most important component; two variants (command block and file example block); header with lang label + filepath label + copy button; syntax highlighting via highlight.js
3. **Collapsible Subsection** — native `<details>`/`<summary>` with CSS chevron rotation; used for reference material (collapsed by default); narrative content stays open
4. **Sidebar Scrollspy** — `IntersectionObserver` highlights active section link as user scrolls; scroll-syncs across tab switches
5. **Callout Boxes** — 4 variants (info, warning, danger, tip) with left border accent; critical for surfacing security warnings (especially `allowed-tools: shell`)
6. **CSS Design System** — 19 layers in the `<style>` block; all values as CSS custom properties; GitHub Primer dark palette

**Build order (from ARCHITECTURE.md):** Base layout → Theme + Typography → Navigation → Code Block → Collapsible → Callout → Content Population → Polish

See [ARCHITECTURE.md](.planning/research/ARCHITECTURE.md) for complete HTML structure, CSS patterns, and full JS implementations.

---

### Critical Pitfalls

All 5 critical pitfalls directly affect guide credibility. The first two will break every command example shown to users.

1. **Wrong CLI identity** — Documenting `gh copilot suggest`/`gh copilot explain` (legacy, deprecated) instead of the modern `copilot` command. Every command example must start with `copilot`, not `gh copilot suggest`. Verify all commands against the CLI command reference before authoring.

2. **Wrong agent file extension** — Using `.md` (cloud agent format) instead of `.agent.md` (CLI agent format). Both live in `.github/agents/` which makes this easy to miss. Add explicit callout: "Copilot CLI agents use `.agent.md`. Cloud agent files use `.md`. Both can coexist."

3. **Wrong skill structure** — Showing skills as flat `.md` files instead of a named directory containing `SKILL.md` (exact case required). Every skill example must open with a directory tree before showing file content.

4. **Missing `allowed-tools: shell` security warning** — Every code example containing `allowed-tools: shell` must have an adjacent `callout--danger` box. Official docs warn it removes the confirmation step for terminal commands and enables arbitrary code execution from untrusted skill sources.

5. **Confusing `gh agent-task` with CLI agents** — `gh agent-task create` creates a GitHub Issue assigned to Copilot (cloud system). `copilot --agent NAME` runs a local CLI agent. Never conflate these in examples.

**Moderate pitfalls to design around:** Stale commands (product is in active preview — add "last verified" date to guide header), external CDN dependencies (smoke test: open file with network disabled), missing copy buttons on all code blocks, and missing custom instructions file discovery order table.

See [PITFALLS.md](.planning/research/PITFALLS.md) for complete pitfall catalog with phase warnings.

---

## Implications for Roadmap

Research strongly suggests a 3-phase delivery: build the technical shell first, populate core content second, complete advanced content and polish third. This order ensures the code block component (the guide's most critical UI element) is validated before content is written into it, and that the most common table-stakes content ships before differentiating content.

### Phase 1: Technical Foundation
**Rationale:** The HTML shell, design system, and JS components must exist before content can be added. Attempting to author content into an unstyled page wastes effort on layout fixes. All component patterns are well-established — no guesswork.
**Delivers:** A fully functional empty shell — working tabs, sidebar scrollspy, code blocks with copy buttons, collapsibles, callout boxes, syntax highlighting — that looks exactly right before a single word of content is written.
**Implements:** All 6 architecture components (CSS Grid shell, Tab Nav, Code Block, Collapsible, Scrollspy, Callouts)
**Uses:** highlight.js 11.11.1 inline, CSS Custom Properties (GitHub Primer dark palette), Vanilla JS event delegation
**Avoids:** Pitfall 8 (CDN dependencies) — smoke test offline before Phase 2; Pitfall 9 (missing copy buttons) — implement in Phase 1 not Phase 2; Pitfall 11 (CSS-only tab limitations) — use JS tabs with ARIA
**Research flag:** Standard patterns — skip phase research. All components are well-documented vanilla JS/CSS with provided implementations.

### Phase 2: Core Content — CLI Intro & Agents
**Rationale:** CLI Intro must exist first because all subsequent examples depend on understanding the `copilot` command. Agents come second because they're the primary differentiating feature of the guide. Table stakes content before differentiators.
**Delivers:** A usable guide with CLI command reference, mode cycling explanation, full annotated agent profile example, built-in agents table, invocation methods, and subagent/orchestration explanation.
**Addresses (from FEATURES.md must-haves):** Core CLI cheat sheet, slash commands, keyboard shortcuts, agent file format, agent locations, built-in agents, agent invocation methods, subagent explanation, `/fleet` mechanics
**Differentiators to include:** Mode cycling visual, built-in agents cheat sheet, naming conflict rules, `--agent` programmatic flag
**Avoids:** Pitfall 1 (wrong CLI identity — verify every command before writing), Pitfall 2 (wrong `.agent.md` extension), Pitfall 5 (`gh agent-task` conflation), Pitfall 14 (frontmatter typos)
**Research flag:** Content accuracy review needed — CLI is actively evolving. Verify all command syntax against `docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference` before populating code blocks.

### Phase 3: Skills Section, Advanced Features & Polish
**Rationale:** Skills have more structural complexity than agents (directory-based format, security warnings, open standard). Fleet+autopilot walkthrough and mode diagram are high-value differentiators but require Phase 2's agent foundation. Polish (responsive, keyboard nav, QA) always comes last.
**Delivers:** Complete guide with Skills section (SKILL.md format, directory structure, script-enabled example, invocation), fleet+autopilot workflow walkthrough, and all polish (mobile responsive sidebar, keyboard nav, "last verified" date header, offline smoke test).
**Addresses (from FEATURES.md):** Full SKILL.md format, skill locations, skills CLI commands, script-enabled skill example, community resources, skills vs agents vs custom instructions, fleet+autopilot walkthrough, mode diagram
**Avoids:** Pitfall 3 (wrong skill structure — always show directory tree first), Pitfall 4 (missing shell security warning — use `callout--danger`), Pitfall 6 (stale commands — add "last verified" date), Pitfall 7 (custom instructions discovery order table), Pitfall 10 (flat skill examples without directory context)
**Defer to collapsed sections:** BYOK env vars (custom model provider), session management deep dive
**Research flag:** Security callout content for `allowed-tools: shell` should be pulled verbatim from official docs to avoid softening the warning.

### Phase Ordering Rationale

- **Shell before content:** Code blocks require the copy button JS and syntax highlighting to be functional before content authors can verify examples look right. Populating content into a broken shell creates rework.
- **CLI Intro before Agents before Skills:** This matches the feature dependency graph from FEATURES.md — CLI commands must exist before agent examples make sense, and agent concepts (subagents, orchestration) must be understood before fleet-based skills workflows.
- **Table stakes before differentiators:** FEATURES.md clearly separates must-haves (Phase 2) from differentiators (Phase 3). This sequencing ensures the guide is useful before it's excellent.
- **Pitfall avoidance baked in:** The two most destructive pitfalls (wrong CLI identity, wrong `.agent.md` extension) are in Phase 2 — they're called out explicitly in the phase spec rather than left to discovery.

### Research Flags

Phases likely needing deeper research during planning:
- **Phase 2:** CLI command syntax — the product is actively evolving. Run a targeted research pass to verify current slash command names and flag syntax before authoring. `/fleet`, `/delegate`, `/on-air` are marked as preview/experimental.
- **Phase 3:** `allowed-tools` security warning — use official docs language verbatim; don't soften.

Phases with standard patterns (skip research-phase):
- **Phase 1:** All vanilla JS/CSS patterns are well-documented with complete implementations provided in ARCHITECTURE.md. No novel decisions required.

---

## Confidence Assessment

| Area | Confidence | Notes |
|------|------------|-------|
| Stack | HIGH | All CDN URLs verified; file sizes confirmed; browser API support documented on MDN |
| Features | HIGH | All content verified against official GitHub Docs (docs.github.com, 2025); URLs confirmed |
| Architecture | HIGH | Established vanilla HTML/CSS/JS patterns; no experimental technology; full implementations provided |
| Pitfalls | HIGH | All critical pitfalls traced to official docs; the `.agent.md` vs `.md` distinction verified in source |

**Overall confidence: HIGH**

### Gaps to Address

- **Prism.js vs highlight.js divergence:** ARCHITECTURE.md recommends Prism.js; STACK.md recommends highlight.js. Both are valid (~same size, ~same quality). **Resolution: Use highlight.js** for its `github-dark` theme. The Prism CSS token class names in ARCHITECTURE.md's theme override section won't apply — use highlight.js's auto-applied classes instead, and inline the `github-dark.min.css` content as-is.
- **CLI feature staleness:** The guide covers features marked "in preview" (`/fleet`, `/delegate`, `gh agent-task`). Add a "Last verified: [date]" note in the guide header and point users to `/help` for current commands. Re-verify command syntax at time of content authoring.
- **Sidebar: authored vs. dynamically generated:** ARCHITECTURE.md mentions two approaches (pre-authored HTML nav lists vs. JS-generated from section headings). For a one-time deliverable, pre-authored lists are simpler and more predictable — recommend this approach in Phase 1.

---

## Sources

### Primary (HIGH confidence)
- `https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference` — all slash commands, flags, keyboard shortcuts
- `https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-custom-agents` — agent format, built-in agents, file locations
- `https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/create-skills` — SKILL.md format, directory structure, security warnings
- `https://docs.github.com/en/copilot/concepts/agents/about-agent-skills` — agent skills open standard, supported directories
- `https://docs.github.com/en/copilot/concepts/agents/copilot-cli/fleet` — `/fleet` mechanics, parallel orchestration, autopilot integration
- `https://cdn.jsdelivr.net/npm/highlight.js@11.11.1/` — file sizes verified, paths confirmed 200 OK
- `https://api.github.com/repos/highlightjs/highlight.js/releases/latest` — confirmed v11.11.1, released 2024-12-25
- MDN — `<details>/<summary>`, `Navigator.clipboard.writeText()`, CSS Custom Properties, Intersection Observer API
- W3C ARIA Authoring Practices Guide — tab pattern
- GitHub Primer design tokens (`primer.style/primitives/colors`) — dark palette verified

### Secondary (MEDIUM confidence)
- `https://cli.github.com/manual/gh_agent-task` — `gh agent-task` as separate cloud-side feature (confirmed not CLI agents)
- `https://cli.github.com/manual/gh_copilot` — `gh copilot` as thin passthrough wrapper for `copilot` command

---

*Research completed: 2025-01-10*
*Ready for roadmap: yes*

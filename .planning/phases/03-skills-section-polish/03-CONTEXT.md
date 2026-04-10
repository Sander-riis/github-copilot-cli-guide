# Phase 3: Skills Section & Polish - Context

**Gathered:** 2026-04-10
**Status:** Ready for planning

<domain>
## Phase Boundary

Complete the Skills tab with accurate, annotated content covering skill creation, directory structure, SKILL.md format, CLI commands, security warnings, and community resources. Add a "last verified" date to the page header. No new CSS or JS — content only, using existing Phase 1 component classes.

</domain>

<decisions>
## Implementation Decisions

### Skills Overview Structure (skills-overview section)
- **D-01:** Open with a directory tree showing `.github/skills/my-skill/SKILL.md` structure before any narrative
- **D-02:** Follow directory tree with a brief inline distinction: "Skills are reusable instruction bundles — unlike agents, they don't get their own context window"
- **D-03:** Include a `callout--info` noting skills as an open standard (Agent Skills spec) that works across Copilot CLI, cloud agents, and VS Code agent mode
- **D-04:** Skill file locations (`.github/skills/` project, `~/.copilot/skills/` personal) in a collapsible with 2-row table — mirrors agent file locations pattern
- **D-05:** Content split mirrors Agents tab: skills-overview has all overview content (what skills are, directory tree, examples, field reference, file locations); skills-reference has CLI commands + invocation syntax + community resources

### SKILL.md Examples
- **D-06:** Annotated basic SKILL.md example uses a fabricated but realistic skill (e.g., "code-review" or "deploy") that showcases all frontmatter fields
- **D-07:** Code block uses filepath header pattern (same as .agent.md example in Agents tab) with inline YAML comments annotating each field
- **D-08:** Collapsible "Frontmatter Field Reference" table covering name, description, license, allowed-tools — mirrors Agents tab pattern
- **D-09:** Script-enabled skill example (SKILL.md + companion .sh file) in a collapsible "Script-Enabled Skill Example" with the danger callout inside

### Skills Reference Structure (skills-reference section)
- **D-10:** CLI commands (`/skills list`, `/skills info`, `/skills add`, `/skills reload`, `/skills remove`, plus `/skill-name` invocation) in a collapsible table
- **D-11:** Community resources (`anthropics/skills`, `github/awesome-copilot`) in a `callout--tip` at the end of skills-reference — natural closing element

### Security Warning
- **D-12:** Strong `callout--danger` warning for `allowed-tools: shell` — inside the script-enabled skill collapsible, right after the shell script code block
- **D-13:** Warning language should be direct about arbitrary command execution risk; do not soften

### Final Polish
- **D-14:** Add "Last verified: YYYY-MM-DD" in muted text to the existing page header

### Agent's Discretion
- Exact wording of the fabricated skill examples (name, description, body content)
- Exact wording of the security warning (as long as it's strong and clear about shell = arbitrary commands)
- Narrative paragraph tone and flow within sections

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Requirements
- `.planning/REQUIREMENTS.md` — full requirement text for SKIL-01 through SKIL-11

### Roadmap & Success Criteria
- `.planning/ROADMAP.md` — Phase 3 success criteria and scope

### Prior Phase Context
- `.planning/phases/02-cli-intro-agents-content/02-CONTEXT.md` — Phase 2 content style decisions, section structure pattern, code example standards
- `.planning/phases/01-technical-foundation/01-UI-SPEC.md` — component classes, CSS tokens, layout specs (filepath header pattern for code blocks)

### Existing Code
- `index.html` — the live file; Skills tab placeholders at lines 1016-1097; page header around line 436

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `.code-block` with `.code-block__filepath` header variant — used for `.agent.md` example in Agents tab, reuse for SKILL.md examples
- `.collapsible` (`<details>/<summary>`) — used throughout CLI and Agents tabs for reference tables
- `.callout--info`, `.callout--warning`, `.callout--danger`, `.callout--tip` — all four variants available
- Copy-to-clipboard buttons auto-initialize on all `.code-block__copy` elements via existing JS
- highlight.js with `bash`, `yaml`, `markdown` languages already inlined

### Established Patterns
- Section opens with 1-2 narrative paragraphs, then structured content (code blocks, tables, collapsibles)
- Agents tab pattern: overview section has annotated example + collapsible field reference + collapsible locations; reference section has table of built-ins + collapsible invocation methods + collapsible orchestration details
- Filepath header on code blocks: `<div class="code-block__filepath">` with file icon SVG + path text

### Integration Points
- Skills tab panel: `<article id="section-skills">` at line 1017
- `skills-overview` section: line 1022 (replace placeholder content)
- `skills-reference` section: line 1075 (replace placeholder content)
- Page header: `<span class="site-header__subtitle">` at line 436 — add "last verified" date nearby

</code_context>

<specifics>
## Specific Ideas

- Directory tree should be the opening element (before any text) — user specifically chose this to lead with the concrete file structure
- Community resources callout goes at the very end of skills-reference as a closing element
- Security warning must be inside the script-enabled collapsible, not standalone — keeps it contextually relevant

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope

</deferred>

---

*Phase: 03-skills-section-polish*
*Context gathered: 2026-04-10*

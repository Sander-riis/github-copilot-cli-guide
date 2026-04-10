# Phase 1: Technical Foundation - Context

**Gathered:** 2026-04-10
**Status:** Ready for planning

<domain>
## Phase Boundary

Build the fully functional, self-contained HTML shell with all UI components working — tabs, sidebar with scrollspy, code blocks with syntax highlighting + copy buttons, collapsibles, and callout boxes — before any content is written. No real guide content in this phase; placeholder text is fine. The output is a single `index.html` file that passes all 5 success criteria from ROADMAP.md.

</domain>

<decisions>
## Implementation Decisions

### File Assembly
- **D-01:** Direct authoring — write `index.html` directly, inline all assets (highlight.js, CSS, JS) by hand. No build script. One file to edit forever.

### Page Header
- **D-02:** Minimal header bar — slim top bar with title and subtitle. Tabs sit just below it. No hero section.
- **D-03:** Page title text: `GitHub Copilot CLI — AI Agents & Skills Guide`

### Sidebar Navigation
- **D-04:** Pre-authored HTML sidebar links — written by hand, not auto-generated from heading tags. Total control, no JS generation needed.
- **D-05:** Scrollspy only — sidebar highlights the active section as the user scrolls. Uses Intersection Observer API. No sticky/collapsible complexity for Phase 1.

### Component Visual Style
- **D-06:** Polished — subtle gradients on callouts, smooth tab transitions (CSS), refined button hover states.
- **D-07:** Callout box style: left border accent. Colored left border per type:
  - `info` → blue
  - `warning` → yellow
  - `danger` → red
  - `tip` → green

### the Agent's Discretion
- Tab transition animation style (fade, slide, or instant) — agent picks what looks best
- Exact color values within the GitHub Primer dark palette
- Copy button icon (SVG clipboard vs "Copy" text vs both)
- Sidebar width and typography sizing

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Stack Decisions
- `.planning/research/STACK.md` — Technology choices: highlight.js 11.11.1 + github-dark theme, vanilla JS (~50 lines), CSS Grid layout, GitHub Primer dark palette
- `.planning/research/ARCHITECTURE.md` — Full HTML structure, CSS architecture, JS component patterns (tabs, scrollspy, copy buttons, collapsibles, callouts), recommended build order

### Requirements
- `.planning/REQUIREMENTS.md` — FOUND-01 through FOUND-08 are the Phase 1 requirements

### Pitfalls to Avoid
- `.planning/research/PITFALLS.md` — Self-contained file pitfalls (CDN links breaking offline use), file size budget (under 200KB), external dependency risks

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- None — greenfield project. No existing components to reuse.

### Established Patterns
- None yet — Phase 1 establishes all patterns that Phases 2 and 3 follow.

### Integration Points
- The HTML shell created here is the single file that Phases 2 and 3 populate with content. Section IDs, tab structure, and sidebar links established here must be stable for later phases.

</code_context>

<specifics>
## Specific Ideas

- Sidebar scrollspy should use `Intersection Observer API` (not scroll event listeners) — mentioned in ARCHITECTURE.md as the right approach
- highlight.js should use the `github-dark` theme specifically — on-brand with GitHub's own production CSS
- All JS must be vanilla, no frameworks — tabs, scrollspy, copy buttons should be ~50 lines total
- File must open offline from a double-click — test by opening `file://` in browser with no network

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 01-technical-foundation*
*Context gathered: 2026-04-10*

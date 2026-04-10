# Phase 3: Skills Section & Polish - Research

**Researched:** 2026-04-10
**Domain:** HTML content authoring — Copilot CLI Skills documentation within an established component system
**Confidence:** HIGH

## Summary

This phase is pure content authoring. The HTML shell, CSS component classes, JavaScript behaviors, and syntax highlighting are all complete from Phase 1. The content patterns (narrative paragraphs → annotated code blocks → collapsible reference tables → callouts) are fully established from Phase 2's Agents tab. Phase 3 replaces placeholder content in two `<section>` elements (`skills-overview` at line 1022 and `skills-reference` at line 1075) and adds a "last verified" date to the page header at line 436.

The current file is 97,865 bytes (~96KB) against a 200KB budget, leaving ~100KB for Skills content — more than enough given the Agents tab's comparable content used roughly 20KB. No new CSS classes, no new JavaScript, no external dependencies are needed.

**Primary recommendation:** Mirror the Agents tab structure exactly — same component classes, same heading patterns, same collapsible/table/callout ordering — with Skills-specific content replacing agent-specific content.

<user_constraints>

## User Constraints (from CONTEXT.md)

### Locked Decisions
- **D-01:** Open with a directory tree showing `.github/skills/my-skill/SKILL.md` structure before any narrative
- **D-02:** Follow directory tree with a brief inline distinction: "Skills are reusable instruction bundles — unlike agents, they don't get their own context window"
- **D-03:** Include a `callout--info` noting skills as an open standard (Agent Skills spec) that works across Copilot CLI, cloud agents, and VS Code agent mode
- **D-04:** Skill file locations (`.github/skills/` project, `~/.copilot/skills/` personal) in a collapsible with 2-row table — mirrors agent file locations pattern
- **D-05:** Content split mirrors Agents tab: skills-overview has all overview content (what skills are, directory tree, examples, field reference, file locations); skills-reference has CLI commands + invocation syntax + community resources
- **D-06:** Annotated basic SKILL.md example uses a fabricated but realistic skill (e.g., "code-review" or "deploy") that showcases all frontmatter fields
- **D-07:** Code block uses filepath header pattern (same as .agent.md example in Agents tab) with inline YAML comments annotating each field
- **D-08:** Collapsible "Frontmatter Field Reference" table covering name, description, license, allowed-tools — mirrors Agents tab pattern
- **D-09:** Script-enabled skill example (SKILL.md + companion .sh file) in a collapsible "Script-Enabled Skill Example" with the danger callout inside
- **D-10:** CLI commands (`/skills list`, `/skills info`, `/skills add`, `/skills reload`, `/skills remove`, plus `/skill-name` invocation) in a collapsible table
- **D-11:** Community resources (`anthropics/skills`, `github/awesome-copilot`) in a `callout--tip` at the end of skills-reference — natural closing element
- **D-12:** Strong `callout--danger` warning for `allowed-tools: shell` — inside the script-enabled skill collapsible, right after the shell script code block
- **D-13:** Warning language should be direct about arbitrary command execution risk; do not soften
- **D-14:** Add "Last verified: YYYY-MM-DD" in muted text to the existing page header

### Agent's Discretion
- Exact wording of the fabricated skill examples (name, description, body content)
- Exact wording of the security warning (as long as it's strong and clear about shell = arbitrary commands)
- Narrative paragraph tone and flow within sections

### Deferred Ideas (OUT OF SCOPE)
None — discussion stayed within phase scope

</user_constraints>

<phase_requirements>

## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| SKIL-01 | Section explains what skills are and how they differ from agents and custom instructions | D-02 inline distinction; narrative opening paragraphs after directory tree |
| SKIL-02 | Skills as an open standard (Agent Skills spec, works across Copilot CLI, cloud agent, VS Code agent mode) is noted | D-03 callout--info in skills-overview |
| SKIL-03 | Skill directory structure is shown: `.github/skills/my-skill/SKILL.md` | D-01 directory tree as first element in skills-overview |
| SKIL-04 | SKILL.md frontmatter fields are documented: `name`, `description`, `license`, `allowed-tools` | D-08 collapsible "Frontmatter Field Reference" table |
| SKIL-05 | A complete annotated SKILL.md example is included — every field and body section explained | D-06/D-07 filepath header code block with inline YAML comments |
| SKIL-06 | A script-enabled skill example is shown: SKILL.md + companion shell script | D-09 collapsible "Script-Enabled Skill Example" |
| SKIL-07 | A security warning is included alongside `allowed-tools: shell` usage | D-12/D-13 callout--danger inside script-enabled collapsible |
| SKIL-08 | Skill file locations are documented: `.github/skills/` (project), `~/.copilot/skills/` (personal) | D-04 collapsible with 2-row table |
| SKIL-09 | All key Skills CLI commands shown: `/skills list`, `/skills info`, `/skills add`, `/skills reload`, `/skills remove` | D-10 collapsible table in skills-reference |
| SKIL-10 | Skill invocation syntax shown: `/skill-name` in a prompt | D-10 included in CLI commands table |
| SKIL-11 | Community skill resources referenced: `anthropics/skills`, `github/awesome-copilot` | D-11 callout--tip at end of skills-reference |

</phase_requirements>

## Project Constraints (from copilot-instructions.md)

The `copilot-instructions.md` describes the project itself (a single-page HTML reference guide) and its core value. The key constraints are:
- **Format:** Single `.html` file — must be fully self-contained
- **Style:** Dark theme, terminal-inspired — code-heavy sections should feel at home
- **Interactivity:** Collapsible sections + tabs; no heavy framework dependencies
- **No external network requests** — everything inlined

## Standard Stack

No new libraries, packages, or tools are needed for this phase. Everything is already inlined in `index.html`:

### Already Available (from Phase 1)
| Component | Status | Usage in Phase 3 |
|-----------|--------|-------------------|
| highlight.js 11.11.1 core | Inlined | Syntax highlighting for SKILL.md YAML examples |
| hljs `yaml` language | Inlined | SKILL.md frontmatter highlighting |
| hljs `bash` language | Inlined | Shell script examples, CLI commands |
| hljs `markdown` language | Inlined | SKILL.md body content if shown as markdown |
| `github-dark` theme CSS | Inlined | Code block color scheme |
| Tab JS + scrollspy + copy-to-clipboard | Inlined | Auto-initializes on new `.code-block__copy` elements |

**No installation step needed. No version changes. No new dependencies.**

## Architecture Patterns

### Content Section Layout (established in Phase 2)

The Agents tab set the canonical pattern. Skills tab MUST follow it:

```
skills-overview section:
├── Directory tree (code-block, bash/plaintext)     ← D-01: first element
├── Narrative paragraphs (p tags)                    ← D-02: distinction from agents
├── callout--info (open standard)                    ← D-03
├── h3 heading: "SKILL.md Format"
├── Narrative paragraph
├── code-block with filepath header (SKILL.md)       ← D-06/D-07
├── collapsible: "Frontmatter Field Reference"       ← D-08
│   └── table (name, description, license, allowed-tools)
├── collapsible: "Script-Enabled Skill Example"      ← D-09
│   ├── code-block with filepath header (SKILL.md)
│   ├── code-block with filepath header (.sh script)
│   └── callout--danger (shell warning)              ← D-12/D-13
└── collapsible: "Skill File Locations"              ← D-04
    └── 2-row table (project, personal)

skills-reference section:
├── h3 heading: "Skills CLI Commands"
├── Narrative paragraph
├── collapsible: "Skills Command Reference"          ← D-10
│   └── table (/skills list, info, add, reload, remove, /skill-name)
└── callout--tip (community resources)               ← D-11
```

### HTML Component Patterns to Reuse

All patterns below are copied verbatim from the Agents tab in `index.html`. The planner/executor should use these as templates, substituting Skills-specific content.

#### Pattern 1: Filepath Header Code Block
**What:** A code block with a file icon + path in the header instead of a language badge.
**When to use:** For the annotated SKILL.md example (D-07) and the script-enabled example files (D-09).
**Source:** Lines 717–763 of `index.html` (the `.agent.md` example).

```html
<div class="code-block" data-lang="yaml">
  <div class="code-block__header">
    <span class="code-block__filepath">
      <svg aria-hidden="true" width="12" height="12" viewBox="0 0 16 16" fill="currentColor">
        <path d="M2 1.75C2 .784 2.784 0 3.75 0h6.586c.464 0 .909.184 1.237.513l2.914 2.914c.329.328.513.773.513 1.237v9.586A1.75 1.75 0 0113.25 16h-9.5A1.75 1.75 0 012 14.25V1.75z"/>
      </svg>
      .github/skills/deploy/SKILL.md
    </span>
    <button class="code-block__copy" aria-label="Copy to clipboard">
      <!-- copy icon SVGs + label (same as all other code blocks) -->
    </button>
  </div>
  <pre class="code-block__pre"><code class="language-yaml">---
name: deploy
description: >
  ...
---</code></pre>
</div>
```

#### Pattern 2: Collapsible with Table
**What:** A `<details>` block containing a reference table.
**When to use:** Field reference (D-08), file locations (D-04), CLI commands (D-10).
**Source:** Lines 766–806 of `index.html` (Frontmatter Field Reference).

```html
<details class="collapsible">
  <summary class="collapsible__trigger">
    <svg class="collapsible__chevron" aria-hidden="true" width="16" height="16" viewBox="0 0 16 16" fill="currentColor">
      <path d="M6.22 3.22a.75.75 0 011.06 0l4.25 4.25a.75.75 0 010 1.06l-4.25 4.25a.75.75 0 01-1.06-1.06L9.94 8 6.22 4.28a.75.75 0 010-1.06z"/>
    </svg>
    Section Title
  </summary>
  <div class="collapsible__body">
    <table>
      <thead><tr><th>Column</th><th>Column</th></tr></thead>
      <tbody><tr><td>...</td><td>...</td></tr></tbody>
    </table>
  </div>
</details>
```

#### Pattern 3: Callout Boxes
**What:** Colored callout with type label.
**When to use:** `callout--info` for open standard note (D-03), `callout--danger` for shell warning (D-12), `callout--tip` for community resources (D-11).

```html
<div class="callout callout--danger">
  <span class="callout__type">Danger</span>
  <p><strong>Security warning content here.</strong></p>
</div>
```

#### Pattern 4: Inline h3 Heading
**What:** Sub-section heading within a `<section>`, using inline `style` for margin.
**When to use:** Before "SKILL.md Format" in overview, before "Skills CLI Commands" in reference.
**Source:** Lines 714, 859, 970 of `index.html`.

```html
<h3 style="margin: 24px 0 8px;">SKILL.md Format</h3>
```

#### Pattern 5: Directory Tree in Code Block
**What:** A plaintext directory tree rendered in a `bash` code block.
**When to use:** D-01 — the opening element of skills-overview.

```html
<div class="code-block" data-lang="bash">
  <div class="code-block__header">
    <span class="code-block__lang">bash</span>
    <button class="code-block__copy" aria-label="Copy to clipboard">
      <!-- standard copy button SVGs -->
    </button>
  </div>
  <pre class="code-block__pre"><code class="language-bash">.github/
└── skills/
    └── my-skill/
        ├── SKILL.md          # required — skill definition
        └── helpers/           # optional — companion scripts
            └── run.sh</code></pre>
</div>
```

### Anti-Patterns to Avoid
- **New CSS classes:** Do NOT create any. Use only existing `.code-block`, `.collapsible`, `.callout--*`, `.code-block__filepath` classes.
- **New JavaScript:** Do NOT add any. The copy-to-clipboard handler auto-initializes on all `.code-block__copy` elements.
- **CDN links or external URLs in `<script>`/`<link>`/`<img>`:** Violates self-containment requirement. Text-only URLs in content (e.g., GitHub repo links) are fine.
- **Deviating from Agents tab layout:** The Skills tab must feel like a natural continuation. Same heading hierarchy, same collapsible depth, same table column patterns.

## Integration Points

### Exact Replacement Targets

| Target | Current Line | Current Content | Action |
|--------|-------------|-----------------|--------|
| `skills-overview` section body | Lines 1023–1073 | Placeholder text + sample components | Replace everything between `<h2>` and closing `</section>` with Skills overview content |
| `skills-reference` section body | Lines 1076–1094 | Placeholder text + sample code block | Replace everything between `<h2>` and closing `</section>` with Skills reference content |
| Page header "last verified" | Line 436 | `<span class="site-header__subtitle">Agents &amp; Skills Guide</span>` | Add muted text element after or within the subtitle area |

### Section Heading Text

The current headings are:
- `Skills — Overview` (line 1023)
- `Skills — Reference` (line 1076)

These should remain as-is (consistent with "Agents Overview" / "Agents Reference" which were renamed in Phase 2, but the Skills headings already follow the pattern).

**Note:** Phase 2 used "Agents Overview" and "Agents Reference" (without em-dash). The Skills placeholders use "Skills — Overview" and "Skills — Reference" (with em-dash). The planner should decide whether to normalize these or leave as-is. This is a minor consistency detail.

### "Last Verified" Date (D-14)

The header area at line 436:
```html
<span class="site-header__subtitle">Agents &amp; Skills Guide</span>
```

Add a muted-text element using `--color-text-secondary` (#8b949e) and 13px font. Example:
```html
<span class="site-header__subtitle">Agents &amp; Skills Guide</span>
<span style="color: var(--color-text-secondary); font-size: 13px; margin-left: 12px;">Last verified: 2026-04-10</span>
```

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Syntax highlighting | Custom regex highlighter | Inlined highlight.js (already present) | It handles YAML, bash, markdown edge cases automatically |
| Copy to clipboard | New JS handler | Existing auto-initialization on `.code-block__copy` | Adding the standard button markup is sufficient; JS picks it up |
| Collapsible sections | Custom JS accordion | Native `<details>/<summary>` with `.collapsible` class | Zero JS, accessible by default, CSS transition on chevron already styled |
| Directory tree visualization | SVG diagram or custom component | Plaintext tree in a bash code block | Simpler, copy-pasteable, matches terminal aesthetic |

## Common Pitfalls

### Pitfall 1: Placeholder Content Not Fully Replaced
**What goes wrong:** Sample callouts, code blocks, or collapsibles from the Phase 1 placeholder remain in the final output alongside real content.
**Why it happens:** The skills-overview section (lines 1022–1073) contains 4 sample callouts, 1 sample code block, and 1 sample collapsible — all placeholders. Easy to miss some during replacement.
**How to avoid:** Replace the ENTIRE body of both sections. Don't try to edit individual elements — replace from the line after `<h2>` down to the line before `</section>`.
**Warning signs:** Seeing "Content for this section will be added in Phase 2" text, or seeing 4 callout variants stacked with identical "Sample callout" text.

### Pitfall 2: File Size Exceeding Budget
**What goes wrong:** Content pushes file past 200KB (FOUND-08).
**Why it happens:** Unlikely given 100KB headroom, but verbose examples or duplicated SVG boilerplate could add up.
**How to avoid:** Monitor file size after each major content block. The Agents tab added ~20KB — Skills should be comparable.
**Warning signs:** File exceeding 150KB after skills-overview (before skills-reference).

### Pitfall 3: Inconsistent Heading Structure
**What goes wrong:** Skills tab uses different heading hierarchy than Agents tab, breaking visual consistency.
**Why it happens:** Agents tab uses `<h2>` for section headings and `<h3 style="margin: ...">` for sub-topics. Easy to accidentally use `<h3>` without the inline margin or use `<h4>` for sub-sub-topics.
**How to avoid:** Stick to the exact pattern: `<h2 class="section-heading">` for the two main sections, `<h3 style="margin: 24px 0 8px;">` (or `32px` for after-table gaps) for sub-topics within them. No `<h4>` or deeper.
**Warning signs:** Visual inconsistency between Agents and Skills tabs when switching.

### Pitfall 4: Incorrect Code Block Language Tags
**What goes wrong:** Code blocks don't get syntax highlighting because the `data-lang` attribute or `language-*` class is wrong.
**Why it happens:** SKILL.md files are YAML frontmatter + Markdown body. The code block should use `data-lang="yaml"` with `class="language-yaml"` (like the .agent.md example). Shell script examples should use `data-lang="bash"` with `class="language-bash"`.
**How to avoid:** Match the Agents tab exactly: `.agent.md` example used `data-lang="yaml"` / `language-yaml`. SKILL.md examples should do the same.
**Warning signs:** Code blocks render as plain white text without color syntax highlighting.

### Pitfall 5: Security Warning Too Weak
**What goes wrong:** The `allowed-tools: shell` danger callout uses hedged language ("be careful", "exercise caution") instead of direct security warning.
**Why it happens:** Natural inclination to soften warnings. Both D-13 and the STATE.md Phase 3 flag explicitly say "do not soften."
**How to avoid:** Use explicit language: "arbitrary command execution", "full shell access", "runs any command the agent constructs." Reference that `allowed-tools: shell` grants the skill permission to execute arbitrary shell commands on the user's machine with the user's permissions.
**Warning signs:** Warning reads like a suggestion rather than a security alert.

### Pitfall 6: Skills Section Headings Don't Match Sidebar
**What goes wrong:** Sidebar nav links (`#skills-overview`, `#skills-reference`) don't match the section IDs.
**Why it happens:** If section IDs are changed during content replacement.
**How to avoid:** Keep `id="skills-overview"` and `id="skills-reference"` exactly as they are. The sidebar links (lines 488-489) point to these IDs and must not break.
**Warning signs:** Clicking sidebar links doesn't scroll to the correct section.

## Content Domain Knowledge

### SKILL.md File Format
**Confidence: HIGH** (sourced from REQUIREMENTS.md which was researched and validated during project setup)

A skill is defined by a `SKILL.md` file inside a named subdirectory:
```
.github/skills/<skill-name>/SKILL.md
```

**Frontmatter fields:**
| Field | Required | Type | Purpose |
|-------|----------|------|---------|
| `name` | Yes | string (lowercase-hyphens) | Skill identifier — used for `/skill-name` invocation |
| `description` | Yes | string | What the skill does — Copilot reads this to decide when to suggest it |
| `license` | No | string | License identifier (e.g., "MIT") — metadata, not enforced |
| `allowed-tools` | No | list of strings | Tools the skill may use; `shell` grants arbitrary command execution |

**Markdown body:** Free-form instructions that become the skill's system prompt context when invoked. Can include step-by-step procedures, constraints, examples, and tool-usage patterns.

### Skills vs Agents vs Custom Instructions
| Feature | Custom Instructions | Skills | Agents |
|---------|-------------------|--------|--------|
| File | `copilot-instructions.md` | `SKILL.md` in subdirectory | `NAME.agent.md` |
| Activation | Always-on | On-demand (`/skill-name`) | On-demand (4 invocation methods) |
| Context window | Shared with session | Shared with session | Isolated (own context window) |
| Scope | Behavioral guidance | Reusable task bundles | Full persona + tool permissions |

### File Locations
| Scope | Path | Available To |
|-------|------|-------------|
| Project (repo) | `.github/skills/` | Anyone with repo access |
| Personal (user) | `~/.copilot/skills/` | Only you, across all repos |

### CLI Commands
| Command | Purpose |
|---------|---------|
| `/skills list` | List all discovered skills |
| `/skills info SKILL` | Show details about a specific skill |
| `/skills add PATH` | Add a skill from a filesystem path |
| `/skills reload` | Reload all skills (pick up changes) |
| `/skills remove SKILL-DIRECTORY` | Remove a previously added skill |
| `/skill-name` | Invoke a skill by its directory name |

### Community Resources
- **`anthropics/skills`** — Anthropic's public skills repository (GitHub)
- **`github/awesome-copilot`** — GitHub's curated list of Copilot resources

### Agent Skills Spec (Open Standard)
Skills follow the Agent Skills specification, which is designed to work across multiple surfaces: Copilot CLI, GitHub cloud agents, and VS Code agent mode. This makes skills portable — the same SKILL.md works regardless of where Copilot runs.

## File Size Budget

| Component | Current Size | Budget | Remaining |
|-----------|-------------|--------|-----------|
| index.html | 97,865 bytes (~96KB) | 200,000 bytes (200KB) | ~102KB |
| Estimated Phase 3 addition | ~15-25KB | — | ~77-87KB after |

The Agents tab content (Phase 2 Plan 2) added approximately 20KB. Skills content should be comparable or slightly smaller (fewer sub-sections). File size is not a concern.

## Code Examples

### Example 1: Fabricated Basic SKILL.md (for D-06/D-07)

The executor should create a realistic skill example. Suggested: a "deploy" or "code-review" skill. Here is a representative content structure:

```yaml
---
name: deploy                              # required — lowercase-hyphens; invoked as /deploy
description: >                            # required — Copilot reads this to decide when to suggest
  Deploys the current project to the configured environment. Reads deployment
  config from deploy.yaml, runs pre-deploy checks, and executes the deploy
  pipeline. Use when asked to deploy, release, or push to production.
license: MIT                              # optional — metadata only; not enforced by Copilot
allowed-tools:                            # optional — restricts which tools this skill may use
  - shell                                 #   ⚠ grants arbitrary command execution
  - view                                  #   read files and directories
---

# Deploy Skill

When invoked, follow this sequence:

1. Read `deploy.yaml` from the project root
2. Run pre-deploy checks: `npm test` and `npm run lint`
3. If checks pass, execute the deploy command from the config
4. Report the deployment status with the environment URL
```

### Example 2: Script-Enabled Skill (for D-09)

Two files shown together in the collapsible:

**SKILL.md:**
```yaml
---
name: db-migrate
description: >
  Runs database migrations using the companion shell script. Use when asked
  to migrate, update schema, or apply pending database changes.
allowed-tools:
  - shell
---

# Database Migration Skill

Run the companion migration script located at `./helpers/migrate.sh`.
Pass the target environment as the first argument.
```

**helpers/migrate.sh:**
```bash
#!/usr/bin/env bash
set -euo pipefail

ENV="${1:-development}"
echo "Running migrations for: $ENV"
npx prisma migrate deploy --schema=./prisma/schema.prisma
echo "Migrations complete."
```

### Example 3: Security Warning (for D-12/D-13)

Strong, direct language — not hedged:
```html
<div class="callout callout--danger">
  <span class="callout__type">Danger</span>
  <p><strong><code>allowed-tools: shell</code> grants arbitrary command execution.</strong> 
  A skill with shell access can run any command on your machine with your user permissions — 
  including modifying files, installing packages, accessing credentials, and making network 
  requests. Only enable shell access for skills you have personally reviewed and trust. 
  Never grant shell access to third-party skills without reading every line of their source.</p>
</div>
```

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| copilot-instructions.md only | Skills + Agents + instructions | Agent Skills spec (2025) | Skills provide modular, reusable, invokable instruction bundles |
| No skill portability | Skills as open standard | Agent Skills spec | Same SKILL.md works across CLI, cloud, VS Code |

## Open Questions

1. **Heading consistency: em-dash vs no em-dash**
   - What we know: Agents tab uses "Agents Overview" / "Agents Reference". Skills placeholders use "Skills — Overview" / "Skills — Reference" (with em-dash).
   - What's unclear: Whether to normalize to one style.
   - Recommendation: The planner should pick one style and apply it. Either keep the em-dash for Skills (it's a minor stylistic difference) or remove it to match Agents. Low impact either way.

2. **Community resource URLs**
   - What we know: D-11 references `anthropics/skills` and `github/awesome-copilot` as GitHub repos.
   - What's unclear: The exact URLs (these are GitHub org/repo paths but should be rendered as text, not clickable links — since the page is offline).
   - Recommendation: Show them as `github.com/anthropics/skills` and `github.com/github/awesome-copilot` in inline `<code>` elements. They can't be clickable (offline page), but showing the path tells the reader where to look.

3. **"Last verified" date value**
   - What we know: D-14 says add the date in muted text to the header.
   - What's unclear: What date to use.
   - Recommendation: Use the date the phase is executed (the actual authoring/verification date). The planner should instruct the executor to use the current date at execution time.

## Sources

### Primary (HIGH confidence)
- `index.html` — Lines 697–1014: Complete Agents tab implementation showing all HTML patterns
- `index.html` — Lines 1016–1097: Current Skills tab placeholder structure
- `.planning/phases/03-skills-section-polish/03-CONTEXT.md` — All 14 locked decisions (D-01 through D-14)
- `.planning/REQUIREMENTS.md` — SKIL-01 through SKIL-11 requirement definitions
- `.planning/phases/01-technical-foundation/01-UI-SPEC.md` — Component specs, CSS tokens, spacing scale

### Secondary (MEDIUM confidence)
- `.planning/phases/02-cli-intro-agents-content/02-CONTEXT.md` — Phase 2 content style decisions and code example standards
- `.planning/STATE.md` — Project decisions and Phase 3 flag about security warning language

### Tertiary (LOW confidence)
- Community resource repository names (`anthropics/skills`, `github/awesome-copilot`) — sourced from requirements; exact current URLs should be verified by the executor if the page were to include them as text

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — no new dependencies; everything already inlined and proven
- Architecture: HIGH — all HTML patterns are established and documented from Phase 2 Agents tab
- Content domain: HIGH — requirements document was researched and validated during project initialization; all content decisions are locked in CONTEXT.md
- Pitfalls: HIGH — based on direct inspection of the codebase and established patterns

**Research date:** 2026-04-10
**Valid until:** 2026-05-10 (stable — content patterns don't change; Copilot CLI Skills spec may evolve but content is authored to current state)

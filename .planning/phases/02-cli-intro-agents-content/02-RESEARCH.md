# Phase 2: CLI Intro & Agents Content — Research

**Researched:** 2026-04-10
**Domain:** HTML content authoring — GitHub Copilot CLI reference (no new code)
**Confidence:** HIGH — all CLI facts drawn from official docs verified in Phase 1 research

---

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions

**Content Style**
- Narrative walkthroughs — every major topic gets a `<section>` with a short explanatory paragraph followed by code blocks or annotated examples
- Not a quick-reference card page; readers should be able to *read* through each section and understand the topic
- Tone: technical but accessible — assumes existing Copilot CLI familiarity, not a beginner

**Section Structure**
- 2 sections per tab — matching the Phase 1 placeholder structure exactly
  - CLI Intro tab: `cli-overview` (commands, mode cycling, shortcuts, power flags) + `cli-reference` (slash commands)
  - Agents tab: `agents-overview` (what agents are, disambiguation, file format) + `agents-reference` (built-ins, invocation, orchestration, fleet)
- Sidebar anchors already exist; content must flow to fit within these 2 sections per tab
- Pack related topics together using `<details>`/`<summary>` collapsibles within each section to avoid visual overload

**`.agent.md` Example**
- Use a GSD-style agent (e.g., `gsd-executor` or similar) as the base for the annotated example
- The example should be a real-looking agent with actual frontmatter (`name`, `description`, `tools`, `mcp-servers`) and a meaningful body — not a toy example
- Annotate every field inline using HTML comments or a split-panel layout
- The example must satisfy AGNT-02 and AGNT-03

**Code Examples**
- Real syntax — use actual copilot CLI commands exactly as they work today
  - `copilot` (bare), `copilot -p "..."`, `copilot -sp "..."`, `copilot --agent NAME`
  - `/fleet`, `/research`, `/plan`, `/model`, `/review`, `/delegate`, `/diff`, `/pr`, `/skills`, `/agent`
  - `@FILENAME`, `!COMMAND`, `Esc`, `Ctrl+L`, `/help`
- No disclaimer notes or "approximate" labels — accuracy is the goal
- All code blocks tagged with correct language (`bash`, `yaml`, `markdown`) for highlight.js

**Carrying Forward from Phase 1**
- Component classes to use: `.code-block`, `.collapsible`, `.callout--info`, `.callout--warning`, `.callout--danger`, `.callout--tip`
- All CSS variables already defined; no new CSS needed unless content genuinely requires it
- No CDN links — everything remains self-contained
- File size budget: ~135KB remaining (current: 69.2KB, limit: 200KB)

### Agent's Discretion

Nothing left to the agent's discretion — all structural and stylistic decisions are locked.

### Deferred Ideas (OUT OF SCOPE)

- Skills tab content — Phase 3
- Hash-based tab state restoration (POL-01) — Phase 3
- Mobile-responsive layout (POL-02) — Phase 3
- Any new CSS or JS — Phase 1 is complete; Phase 2 is content only
- v2 requirements (ADV-01–04) — future milestone
</user_constraints>

---

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| CLI-01 | Narrative explanation of how Copilot CLI works (context for power users) | FEATURES.md §CLI intro; copilot command is standalone terminal AI agent |
| CLI-02 | Key command-line patterns: `copilot`, `copilot -p "..."`, `copilot -sp "..."`, `copilot --agent NAME` | Verified in FEATURES.md + PITFALLS.md pitfall 1 |
| CLI-03 | Mode cycling: standard → plan → autopilot via `Shift+Tab` | FEATURES.md §Modes |
| CLI-04 | Key shortcuts: `@FILENAME`, `!COMMAND`, `Esc`, `Ctrl+L`, `/help` | FEATURES.md §keyboard shortcuts; CLI reference docs |
| CLI-05 | Slash commands covered narratively (10 commands minimum) | Verified list in FEATURES.md; all confirmed in CLI reference |
| CLI-06 | Power-user flags: `--allow-all-tools`, `--autopilot`, `--agent`, `--resume`, `--allow-tool`, `--deny-tool` | FEATURES.md §flags section |
| CLI-07 | `/research` agent's special status — only invokable via `/research`, never auto-triggered | FEATURES.md built-in agents table; research row |
| AGNT-01 | What agents are vs regular Copilot prompts (separate context window, persistent persona) | FEATURES.md §agents; official docs about-custom-agents |
| AGNT-02 | Agent profile format: `.agent.md` extension, YAML frontmatter with `name`, `description`, `tools`, `mcp-servers` | FEATURES.md §Agent Profile Frontmatter Fields (HIGH confidence) |
| AGNT-03 | Complete annotated `.agent.md` example — every field explained inline | No real file in `.github/agents/` — must author GSD-style example |
| AGNT-04 | Agent file locations: `.github/agents/` (repo), `~/.copilot/agents/` (user), `/agents/` in `.github-private` (org/enterprise) | FEATURES.md §locations table |
| AGNT-05 | All five built-in agents with capabilities and constraints: `explore`, `task`, `general-purpose`, `code-review`, `research` | FEATURES.md §Built-in Agents table (verified HIGH) |
| AGNT-06 | All four agent invocation methods: `/agent NAME`, prompt mention, trigger-word inference, `--agent NAME` flag | FEATURES.md + invoke-custom-agents docs |
| AGNT-07 | Subagent orchestration: main agent as orchestrator, subagents with isolated context windows | FEATURES.md §Subagent/orchestration |
| AGNT-08 | `/fleet` command: how main agent decomposes tasks and runs subagents in parallel | FEATURES.md §fleet; fleet concept docs |
| AGNT-09 | Naming conflict resolution: user-level overrides repo-level, repo-level overrides org-level | FEATURES.md naming conflict resolution rules |
| AGNT-10 | Disambiguation callout early in section: CLI agents (`.agent.md`) vs cloud agents (GitHub.com UI) | PITFALLS.md pitfall 2 — critical distinction |
</phase_requirements>

---

## Summary

Phase 2 is pure content authoring — no new CSS, no new JavaScript, no new libraries. Every HTML pattern, CSS component class, and interactive behavior already exists in the Phase 1 shell. The work is replacing placeholder paragraphs inside four `<section>` elements with real, accurate, well-structured content using the existing `.code-block`, `.collapsible`, and `.callout--*` components.

The primary risk is **content accuracy**, not implementation complexity. The GitHub Copilot CLI is an actively evolving product, and two critical pitfalls threaten guide credibility: (1) using `gh copilot suggest/explain` syntax (deprecated) instead of the modern `copilot` command, and (2) using `.md` extension (cloud agent format) instead of `.agent.md` (CLI agent format). All technical facts in this research document are drawn from official GitHub Docs verified during Phase 1.

The file size budget is comfortable: current file is **70,853 bytes (69.2 KB)** with **130.8 KB remaining** before the 200 KB limit. The 17 requirements across 4 sections will generate approximately 30–50 KB of HTML content, leaving 80+ KB for Phase 3.

**Primary recommendation:** Author content sections in order — `cli-overview` → `cli-reference` → `agents-overview` → `agents-reference`. Track file size after each section to stay within budget.

---

## Standard Stack

### Core (No Change)
This phase adds zero new libraries. All components are Phase 1 deliverables.

| Component | Class/API | Purpose | Status |
|-----------|-----------|---------|--------|
| Code blocks | `.code-block` + `.code-block__header/.pre` | Show CLI commands, agent files, YAML | ✅ Phase 1 |
| Collapsibles | `.collapsible` + `<details>/<summary>` | Group reference material without overloading | ✅ Phase 1 |
| Callout boxes | `.callout .callout--{info/warning/danger/tip}` | Highlight disambiguation, tips, warnings | ✅ Phase 1 |
| Syntax highlighting | highlight.js 11.11.1, `bash`/`yaml`/`markdown` langs | Color code blocks | ✅ Phase 1 |
| Tables | Plain `<table>` with Phase 1 CSS | Show built-in agents, shortcuts | ✅ Phase 1 |

**Installation:** None required.

---

## Architecture Patterns

### Sections Already in index.html (do NOT restructure)

```
<article id="section-cli-intro">           ← CLI tab panel
  <section id="cli-overview">              ← Replace placeholder content here
  <section id="cli-reference">             ← Replace placeholder content here

<article id="section-agents">             ← Agents tab panel
  <section id="agents-overview">           ← Replace placeholder content here
  <section id="agents-reference">          ← Replace placeholder content here
```

The sidebar nav items are **already correct** (`#cli-overview`, `#cli-reference`, `#agents-overview`, `#agents-reference`). Do not add new sections or change IDs — the scrollspy is anchored to these.

### Requirement-to-Section Mapping

| Section | Requirements | Strategy |
|---------|-------------|----------|
| `cli-overview` | CLI-01, CLI-02, CLI-03, CLI-04, CLI-06 | Lead with narrative intro (CLI-01), then command patterns (CLI-02), mode cycling (CLI-03), keyboard shortcuts (CLI-04), power flags (CLI-06) using collapsibles for the denser reference material |
| `cli-reference` | CLI-05, CLI-07 | Slash commands table + narrative (CLI-05); special callout on `/research` status (CLI-07) |
| `agents-overview` | AGNT-01, AGNT-10, AGNT-02, AGNT-03, AGNT-04, AGNT-09 | Open with disambiguation callout (AGNT-10 first), then "what agents are" (AGNT-01), file format (AGNT-02), annotated example (AGNT-03), locations (AGNT-04), naming conflicts (AGNT-09) |
| `agents-reference` | AGNT-05, AGNT-06, AGNT-07, AGNT-08 | Built-in agents table (AGNT-05), invocation methods (AGNT-06), orchestration narrative (AGNT-07), `/fleet` explanation (AGNT-08) |

### Pattern 1: Narrative Section with Code Block

```html
<!-- Source: Phase 1 shell — exact pattern to replicate -->
<section id="cli-overview" class="content-section">
  <h2 class="section-heading">CLI Overview</h2>
  <p>Introductory narrative paragraph...</p>
  <p>Second paragraph if needed...</p>

  <div class="code-block" data-lang="bash">
    <div class="code-block__header">
      <span class="code-block__lang">bash</span>
      <button class="code-block__copy" aria-label="Copy to clipboard">
        <svg class="icon icon--copy" aria-hidden="true" width="14" height="14" viewBox="0 0 16 16" fill="currentColor">
          <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 010 1.5h-1.5a.25.25 0 00-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 00.25-.25v-1.5a.75.75 0 011.5 0v1.5A1.75 1.75 0 019.25 16h-7.5A1.75 1.75 0 010 14.25v-7.5z"/><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0114.25 11h-7.5A1.75 1.75 0 015 9.25v-7.5zm1.75-.25a.25.25 0 00-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 00.25-.25v-7.5a.25.25 0 00-.25-.25h-7.5z"/>
        </svg>
        <svg class="icon icon--check icon--hidden" aria-hidden="true" width="14" height="14" viewBox="0 0 16 16" fill="currentColor">
          <path d="M13.78 4.22a.75.75 0 010 1.06l-7.25 7.25a.75.75 0 01-1.06 0L2.22 9.28a.75.75 0 011.06-1.06L6 10.94l6.72-6.72a.75.75 0 011.06 0z"/>
        </svg>
        <span class="code-block__copy-label">Copy code</span>
      </button>
    </div>
    <pre class="code-block__pre"><code class="language-bash">copilot -p "explain the auth flow in src/auth/"</code></pre>
  </div>
</section>
```

### Pattern 2: Code Block with File Path Label

Use for `.agent.md` file examples (the filepath variant):

```html
<div class="code-block" data-lang="yaml">
  <div class="code-block__header">
    <!-- filepath instead of lang badge -->
    <span class="code-block__filepath">
      <svg aria-hidden="true" width="12" height="12" viewBox="0 0 16 16" fill="currentColor">
        <path d="M2 1.75C2 .784 2.784 0 3.75 0h6.586c.464 0 .909.184 1.237.513l2.914 2.914c.329.328.513.773.513 1.237v9.586A1.75 1.75 0 0113.25 16h-9.5A1.75 1.75 0 012 14.25V1.75z"/>
      </svg>
      .github/agents/gsd-executor.agent.md
    </span>
    <button class="code-block__copy" aria-label="Copy to clipboard">
      <!-- same SVG copy/check icons as above -->
    </button>
  </div>
  <pre class="code-block__pre"><code class="language-yaml">---
name: gsd-executor
...
</code></pre>
</div>
```

### Pattern 3: Collapsible for Reference Material

Use collapsibles for: the full slash command list, power flags reference, invocation methods detail.

```html
<details class="collapsible">
  <summary class="collapsible__trigger">
    <svg class="collapsible__chevron" aria-hidden="true" width="16" height="16" viewBox="0 0 16 16" fill="currentColor">
      <path d="M6.22 3.22a.75.75 0 011.06 0l4.25 4.25a.75.75 0 010 1.06l-4.25 4.25a.75.75 0 01-1.06-1.06L9.94 8 6.22 4.28a.75.75 0 010-1.06z"/>
    </svg>
    Slash Command Reference
  </summary>
  <div class="collapsible__body">
    <!-- table or list content -->
  </div>
</details>
```

### Pattern 4: Disambiguation / Warning Callout

AGNT-10 must open `agents-overview` with a `callout--info` distinguishing CLI agents from cloud agents:

```html
<div class="callout callout--info">
  <span class="callout__type">Info</span>
  <p><strong>Two agent systems share the same directory.</strong> Copilot CLI agents use the
  <code>.agent.md</code> double extension and are invoked locally from your terminal.
  GitHub cloud agents (GitHub.com Issues UI) use plain <code>.md</code> files.
  Both live in <code>.github/agents/</code> — the extension is the only visual difference.</p>
</div>
```

### Pattern 5: Data Table (built-in agents)

Phase 1 CSS already styles `<table>` elements:

```html
<table>
  <thead>
    <tr>
      <th>Agent</th>
      <th>Invoked By</th>
      <th>Parallel?</th>
      <th>Changes Files?</th>
      <th>Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>explore</code></td>
      <td>Auto or subagent</td>
      <td>✅ Yes (read-only)</td>
      <td>❌ No</td>
      <td>Fast codebase search; read-only GitHub MCP access</td>
    </tr>
    <!-- … -->
  </tbody>
</table>
```

### Anti-Patterns to Avoid

- **Creating new `<section>` elements**: The sidebar only has 2 links per tab. New sections won't be navigable and will confuse the scrollspy.
- **Adding new CSS rules**: No `<style>` additions — Phase 2 is HTML content only.
- **Changing section IDs**: `cli-overview`, `cli-reference`, `agents-overview`, `agents-reference` are wired to the sidebar nav and scrollspy JS.
- **Using `gh copilot suggest` or `gh copilot explain`**: These are legacy commands from a deprecated product (see Critical Pitfalls below).
- **Using `.md` extension for CLI agent files**: Cloud agents use `.md`; CLI agents require `.agent.md`.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Copy button | New clipboard JS | Existing `initCopyButtons()` — already handles all `.code-block__copy` buttons via event delegation | It's already working; every new code block with `.code-block__copy` gets handled automatically |
| Syntax highlighting | Per-block JS calls | Existing `hljs.highlightAll()` on DOMContentLoaded | Highlight.js auto-targets all `<code class="language-*">` elements |
| Collapsible animation | JS animation | Native `<details>` + existing CSS chevron rotation | Already implemented; zero JS required |
| Callout icons | New SVG icons | Text-only type labels (`.callout__type`) | Phase 1 spec explicitly calls for type labels, not icons |

---

## Verified Technical Content

### CLI Commands (HIGH confidence — verified against official docs in Phase 1 research)

```bash
# Bare interactive session
copilot

# Non-interactive with prompt
copilot -p "refactor src/auth/login.ts to use async/await"

# Short prompt alias
copilot -sp "what does this repo do?"

# Invoke a specific custom agent
copilot --agent security-auditor "review src/ for injection vulnerabilities"

# Autopilot mode from the start
copilot --autopilot -p "add comprehensive unit tests to src/utils/"

# Resume a previous session
copilot --resume

# Allow specific tool without per-call confirmation
copilot --allow-tool='shell(git log)' -p "summarize recent changes"

# Allow all tools (use with trusted tasks only)
copilot --allow-all-tools -p "run the test suite and fix any failures"
```

### Mode Cycling (HIGH confidence)

Three modes, cycled with `Shift+Tab` during an interactive session:

| Mode | Behavior |
|------|----------|
| **Standard** | Normal interactive — asks for confirmation before tool use |
| **Plan** | Creates a written implementation plan first; user reviews before any execution |
| **Autopilot** | Autonomous mode — auto-confirms tool requests; combines well with `/fleet` |

### Keyboard Shortcuts (HIGH confidence)

| Shortcut | Effect |
|----------|--------|
| `Shift+Tab` | Cycle through Standard → Plan → Autopilot modes |
| `@FILENAME` | Include a file's content in the current prompt context |
| `!COMMAND` | Pass a shell command through without Copilot interpreting it |
| `Esc` | Cancel the current operation |
| `Ctrl+L` | Clear the terminal screen (session continues) |
| `/help` | List all available slash commands for the current session |

### Slash Commands (HIGH confidence — from CLI reference docs)

| Command | Purpose | Notes |
|---------|---------|-------|
| `/plan` | Draft an implementation plan before executing | Switches to plan mode |
| `/fleet` | Decompose task and run subagents in parallel | Requires orchestrator-capable session |
| `/model` | Switch the active AI model mid-session | e.g., `/model claude-3-5-sonnet` |
| `/review` | Invoke the `code-review` agent on current context | Read-only; bugs/security only |
| `/delegate` | Hand off a subtask to a subagent | Preview feature |
| `/research` | Invoke the `research` agent (web fetch/search) | **Only way to invoke `research`** |
| `/diff` | Show current working tree diff within session | |
| `/pr` | Create a PR from current changes | |
| `/skills` | Manage and list agent skills | `/skills list`, `/skills info NAME` |
| `/agent` | Switch to or invoke a specific agent | `/agent gsd-executor` |

> **`/research` is special:** The `research` built-in agent can only be invoked explicitly via `/research`. It is the only built-in that is never auto-triggered by trigger-word inference. (Satisfies CLI-07)

### Power Flags (HIGH confidence)

| Flag | Effect |
|------|--------|
| `--allow-all-tools` | Skip all tool confirmation prompts for the session |
| `--autopilot` | Start session in autopilot mode |
| `--agent NAME` | Launch session with a specific agent active (NAME = filename without `.agent.md`) |
| `--resume` | Resume the most recent session |
| `--allow-tool='TOOL(ARGS)'` | Pre-approve one specific tool call pattern |
| `--deny-tool='TOOL'` | Block a specific tool for the session |

### Agent File Format (HIGH confidence — verified against official GitHub Docs)

**File extension:** `.agent.md` (double extension — NOT plain `.md`)
**Location example:** `.github/agents/gsd-executor.agent.md`
**Invocation:** `copilot --agent gsd-executor` (filename without `.agent.md`)

```yaml
---
name: gsd-executor           # optional; filename used as ID if omitted
description: >               # required — tells Copilot WHEN to invoke this agent
  Executes GSD workflow phases. Use when asked to run a GSD command,
  execute a planned phase, or work through a task list.
tools:                        # optional; defaults to all available tools
  - shell
  - write
  - view
mcp-servers:                  # optional; attach MCP servers to this agent's context
  - name: github
    type: http
    url: https://api.githubcopilot.com/mcp/
---

You are the GSD (Get Shit Done) executor agent...
[Markdown body becomes the agent's system prompt]
```

**Critical distinction** (AGNT-10 — must be shown as callout):

| System | File format | Invocation surface |
|--------|-------------|-------------------|
| Copilot CLI custom agents | `.github/agents/NAME.agent.md` | Terminal: `copilot --agent NAME` or `/agent NAME` |
| Copilot cloud agents | `.github/agents/NAME.md` | GitHub.com Issues/PRs UI |

### Built-in Agents (HIGH confidence — FEATURES.md §Built-in Agents, verified from invoke-custom-agents docs)

| Agent | Invoked By | Parallel? | Changes Files? | Constraints |
|-------|-----------|-----------|----------------|-------------|
| `explore` | Auto-inference or subagent | ✅ Yes (read-only) | ❌ No | Fast codebase search; read-only GitHub MCP access |
| `task` | Auto-inference or subagent | Context-dependent | Via commands | Runs builds/tests/linters; brief success output, full failure output |
| `general-purpose` | Auto-inference or subagent | ✅ When appropriate | ✅ Yes | Full capabilities; separate context window; same tools as main agent |
| `code-review` | Auto-inference, `/review`, or subagent | ✅ Yes | ❌ No | Bugs/security/race conditions/memory leaks only; never style; never modifies files |
| `research` | `/research` **ONLY** | ❌ No | ❌ No | Web fetch/search; **never auto-triggered**; only explicit invocation via `/research` |

### Agent Invocation Methods (HIGH confidence — from AGNT-06)

All four ways to invoke a custom agent:

1. **Slash command:** `/agent gsd-executor` — explicit name, switches active agent immediately
2. **Prompt mention:** "Use the security-auditor agent to review this PR" — Copilot resolves by name
3. **Trigger-word inference:** Copilot reads `description:` frontmatter and auto-routes based on keywords in your prompt
4. **CLI flag:** `copilot --agent gsd-executor` — programmatic/scripting use, starts session with agent pre-selected

### Agent File Locations (HIGH confidence — AGNT-04)

| Scope | Path | Who Can Use |
|-------|------|-------------|
| Repository | `.github/agents/NAME.agent.md` | Anyone with repo access |
| User (personal) | `~/.copilot/agents/NAME.agent.md` | Only you, across all repos |
| Org/Enterprise | `.github-private/agents/NAME.agent.md` | Org members (requires enterprise plan) |

**Naming conflict resolution (AGNT-09):** User-level agents override repo-level agents override org-level agents. Same filename = user wins.

### Subagent Orchestration (HIGH confidence — AGNT-07, AGNT-08)

- **Main agent** acts as orchestrator — it reads the task, decomposes it, decides which subagents to invoke
- **Subagents** each get an **isolated context window** — they don't share memory with each other or the main agent
- **Why isolation matters:** Subagents can run truly in parallel without context contamination; each subagent sees only what it needs
- **`/fleet` command:** Main agent analyzes the full task → identifies parallelizable subtasks → dispatches subagents simultaneously → synthesizes results. Best combined with autopilot mode (`Shift+Tab` → Autopilot, then `/fleet "your task"`)

---

## GSD-Style Agent Example (for AGNT-03)

Since `.github/agents/` is empty in this project, the annotated example must be authored from scratch. Base it on a realistic GSD-style executor agent. This is the pattern copilot-instructions.md describes for GSD commands:

```markdown
---
name: gsd-executor
description: >
  Executes GSD workflow phases following the .planning/ directory conventions.
  Use when asked to run a GSD execute command, work through a phase plan, or
  complete a task list from a PLAN.md file.
tools:
  - shell
  - view
  - write
  - search
mcp-servers:
  - name: github
    type: http
    url: https://api.githubcopilot.com/mcp/
---

# GSD Executor Agent

You are the execution agent for the Get Shit Done (GSD) workflow. Your role
is to carry out tasks defined in `.planning/phases/*/PLAN.md` files with
precision and in the correct order.

## Core Principles

- Read the full PLAN.md before starting any task
- Execute tasks in wave order — never skip ahead
- Commit after each completed task if `commit_docs: true` in config.json
- Stop and report blockers immediately rather than improvising

## Context

Project planning lives in `.planning/`. Phase plans are in
`.planning/phases/{phase}-{slug}/`. The copilot-instructions.md
establishes project-wide constraints that override any plan detail.
```

Each frontmatter field needs an inline annotation in the rendered HTML. Use an approach that places the annotation as a comment-style note after each field in the code block, or use a side-by-side prose + code layout within the collapsible body.

---

## File Size Budget

| Item | Bytes | Notes |
|------|-------|-------|
| Current `index.html` | 70,853 | Measured 2026-04-10 |
| Limit | 204,800 | FOUND-08 requirement |
| **Available for Phase 2** | **133,947** | ~130.8 KB |
| Estimated Phase 2 content | ~35,000–50,000 | 17 requirements × avg 2–3 KB HTML |
| **After Phase 2 estimate** | **~83,947–98,947** | ~82–97 KB total |
| Phase 3 headroom | ~105,853–120,853 | Well within limit |

**Content density guidance:**
- Each `.code-block` wrapper adds ~600 bytes of HTML markup (SVG icons + structure)
- Each `.collapsible` wrapper adds ~250 bytes
- Each `.callout` adds ~150 bytes
- A `<p>` paragraph of 60 words adds ~400 bytes
- A 10-row `<table>` adds ~800 bytes

At a comfortable pace: cli-overview (~10–15 KB), cli-reference (~8–12 KB), agents-overview (~10–16 KB), agents-reference (~8–12 KB) = 36–55 KB total addition. **Budget is safe.**

---

## Common Pitfalls

### Pitfall 1: Wrong CLI Identity — Using Legacy `gh copilot suggest/explain`
**What goes wrong:** Code blocks show `gh copilot suggest "..."` or `gh copilot explain "..."` — these are a deprecated legacy product.
**Why it happens:** Training data and older blog posts prominently feature the legacy commands; LLM generation defaults to them.
**How to avoid:** Every command must start with `copilot`, not `gh copilot suggest/explain`. The modern CLI is a standalone terminal agent (`copilot` command).
**Warning sign:** Any code block containing `gh copilot suggest` or `gh copilot explain`.

### Pitfall 2: Wrong Agent File Extension
**What goes wrong:** Agent file example shows `security-expert.md` instead of `security-expert.agent.md`.
**Why it happens:** Cloud agents (GitHub.com UI) use `.md`; CLI agents require `.agent.md`. Both live in `.github/agents/`.
**How to avoid:** Always use `.agent.md` for CLI agent files. Include the AGNT-10 disambiguation callout at the top of agents-overview.
**Warning sign:** Any agent file path ending in `.md` that isn't clearly identified as a cloud agent.

### Pitfall 3: Missing Copy Button SVG in New Code Blocks
**What goes wrong:** New code blocks are added without the dual-SVG copy/check icon pattern, so the JS copy handler still fires but the visual feedback fails.
**Why it happens:** Copy button structure requires two `<svg>` elements — one `icon--copy` (visible), one `icon--check icon--hidden` (revealed on success). If either is missing, the toggle logic silently breaks.
**How to avoid:** Copy the full code block HTML pattern from the existing sections in index.html — do not simplify.
**Warning sign:** Code block that has a `<button class="code-block__copy">` but is missing `icon--copy`/`icon--check` SVGs.

### Pitfall 4: Adding Content Outside Existing `<section>` Elements
**What goes wrong:** Author adds a third section or sub-`<section>` to a tab panel, which the sidebar nav doesn't have a link for — scrollspy fires on it but no sidebar link highlights.
**How to avoid:** All content for each tab must fit within exactly 2 `<section>` elements. Use collapsibles within sections to manage density instead of adding sections.
**Warning sign:** A new `<section id="new-id" class="content-section">` not in the original index.html.

### Pitfall 5: Bare Code in `<pre>` Without highlight.js Class
**What goes wrong:** Code block renders without syntax coloring because highlight.js auto-detection is less reliable than explicit class annotation.
**How to avoid:** Every `<code>` inside a `.code-block__pre` must have `class="language-bash"`, `class="language-yaml"`, or `class="language-markdown"`. Never leave it as bare `<code>`.

---

## Code Examples

### Full Code Block (bash) — copy-paste ready
```html
<div class="code-block" data-lang="bash">
  <div class="code-block__header">
    <span class="code-block__lang">bash</span>
    <button class="code-block__copy" aria-label="Copy to clipboard">
      <svg class="icon icon--copy" aria-hidden="true" width="14" height="14" viewBox="0 0 16 16" fill="currentColor">
        <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 010 1.5h-1.5a.25.25 0 00-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 00.25-.25v-1.5a.75.75 0 011.5 0v1.5A1.75 1.75 0 019.25 16h-7.5A1.75 1.75 0 010 14.25v-7.5z"/><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0114.25 11h-7.5A1.75 1.75 0 015 9.25v-7.5zm1.75-.25a.25.25 0 00-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 00.25-.25v-7.5a.25.25 0 00-.25-.25h-7.5z"/>
      </svg>
      <svg class="icon icon--check icon--hidden" aria-hidden="true" width="14" height="14" viewBox="0 0 16 16" fill="currentColor">
        <path d="M13.78 4.22a.75.75 0 010 1.06l-7.25 7.25a.75.75 0 01-1.06 0L2.22 9.28a.75.75 0 011.06-1.06L6 10.94l6.72-6.72a.75.75 0 011.06 0z"/>
      </svg>
      <span class="code-block__copy-label">Copy code</span>
    </button>
  </div>
  <pre class="code-block__pre"><code class="language-bash">copilot -p "refactor src/auth/login.ts to use async/await"</code></pre>
</div>
```

### Callout — Info variant
```html
<div class="callout callout--info">
  <span class="callout__type">Info</span>
  <p>Content here.</p>
</div>
```

### Callout — Warning variant (for /delegate, /fleet preview status)
```html
<div class="callout callout--warning">
  <span class="callout__type">Warning</span>
  <p><strong>/fleet and /delegate are preview features</strong> and may change. Verify behavior against <code>/help</code> in your active session.</p>
</div>
```

### Collapsible — Reference Details
```html
<details class="collapsible">
  <summary class="collapsible__trigger">
    <svg class="collapsible__chevron" aria-hidden="true" width="16" height="16" viewBox="0 0 16 16" fill="currentColor">
      <path d="M6.22 3.22a.75.75 0 011.06 0l4.25 4.25a.75.75 0 010 1.06l-4.25 4.25a.75.75 0 01-1.06-1.06L9.94 8 6.22 4.28a.75.75 0 010-1.06z"/>
    </svg>
    Power Flags Reference
  </summary>
  <div class="collapsible__body">
    <table>...</table>
  </div>
</details>
```

---

## Environment Availability

Step 2.6: SKIPPED — Phase 2 is pure HTML content authoring with no external dependencies, no CLI tools, no databases, and no services beyond a text editor.

---

## State of the Art

| Old Approach | Current Approach | Impact |
|--------------|------------------|--------|
| `gh copilot suggest "..."` | `copilot -p "..."` | Every CLI example must use new syntax |
| `.github/agents/NAME.md` (cloud) | `.github/agents/NAME.agent.md` (CLI) | File extension is the only visual difference — critical to show correctly |
| Separate prompt file | `.agent.md` frontmatter body = system prompt | Agent instructions are the Markdown body, not a separate file |

**Actively in preview (mark with warning callout):**
- `/fleet` — parallel subagent orchestration
- `/delegate` — explicit subagent handoff
- These commands are real and functional but syntax/behavior may change

---

## Open Questions

1. **`mcp-servers` frontmatter syntax**
   - What we know: The field exists and accepts a list; can reference built-in GitHub MCP server
   - What's unclear: Whether `type: http` + `url:` is the only syntax, or if there are shorthand forms (e.g., `github` as a string)
   - Recommendation: Show the most conservative/verbose form (with `name:`, `type:`, `url:`) so examples remain accurate even if shorthand forms are added later

2. **`copilot -sp` vs `copilot -s -p` vs `copilot --short-prompt`**
   - What we know: CONTEXT.md lists `copilot -sp "..."` as a real pattern
   - What's unclear: Whether `-sp` is a combined flag or if `-s` has a separate meaning (short output?)
   - Recommendation: Show it as documented in CONTEXT.md; mark with "see `/help` for all flags" note

3. **Preview command language**
   - What we know: STATE.md flags `/fleet` and `/delegate` as "preview/experimental"
   - What's unclear: Whether to add "preview" labels to these in the content or treat them as stable
   - Recommendation: Add a single `callout--warning` in the `/fleet` and `/delegate` entries noting preview status without over-hedging every example

---

## Sources

### Primary (HIGH confidence)
- `.planning/research/FEATURES.md` — Complete CLI command reference, agent frontmatter fields, built-in agents table, invocation methods; verified against official GitHub Docs (docs.github.com)
- `.planning/research/PITFALLS.md` — Content accuracy risks; `.agent.md` vs `.md` distinction, legacy command traps
- `.planning/research/SUMMARY.md` — Project-level research synthesis, phase 2 content strategy
- `index.html` — Phase 1 shell; actual HTML patterns to replicate for code blocks, collapsibles, callouts
- `.planning/phases/02-cli-intro-agents-content/02-CONTEXT.md` — Locked decisions for this phase

### Secondary (MEDIUM confidence)
- `copilot-instructions.md` — GSD workflow structure; informs the gsd-executor agent example
- `.planning/phases/01-technical-foundation/01-UI-SPEC.md` — Component specs, CSS tokens, exact class names

---

## Metadata

**Confidence breakdown:**
- CLI command accuracy: HIGH — verified against official docs in Phase 1 research, all sources cited in FEATURES.md
- Agent file format: HIGH — frontmatter fields and `.agent.md` extension verified in official docs
- Built-in agents table: HIGH — five agents documented in official invoke-custom-agents reference
- HTML patterns: HIGH — copied from live Phase 1 index.html (the actual production code)
- Preview feature status (`/fleet`, `/delegate`): MEDIUM — functional but documented as in-preview

**Research date:** 2026-04-10
**Valid until:** 2026-05-10 (CLI is actively evolving — re-verify slash command names before authoring)

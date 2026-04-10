# Domain Pitfalls

**Domain:** Single-file interactive HTML developer guide — GitHub Copilot CLI agents & skills  
**Researched:** 2025-01-27  
**Sources:** GitHub Copilot CLI official docs (docs.github.com), gh CLI manual (cli.github.com)

---

## Critical Pitfalls

Mistakes that cause rewrites, user confusion, or broken examples.

---

### Pitfall 1: Wrong CLI Identity — Documenting the Old `gh copilot suggest/explain` Commands

**What goes wrong:**  
Author conflates the legacy `gh copilot suggest` / `gh copilot explain` extension (GitHub Copilot for CLI, deprecated/superseded) with the modern GitHub Copilot CLI (`copilot` command, `@github/copilot` npm package). The guide shows commands like `gh copilot suggest "list files"` or `gh copilot explain "grep -r"` which belong to a completely different product.

**Why it happens:**  
Training data and older blog posts/guides prominently feature the legacy extension. LLM-assisted content generation will confidently reproduce these stale commands.

**Consequences:**  
Every command example in the "CLI intro" section is wrong. Users following along cannot reproduce results. The entire guide loses credibility on first use.

**What the current CLI actually is:**  
- Standalone terminal-native AI agent installed via:
  - `npm install -g @github/copilot` (cross-platform, requires Node 22+)
  - `winget install GitHub.Copilot` (Windows)
  - `brew install copilot-cli` (macOS/Linux)
- Launched with: `copilot` (interactive) or `copilot -p "your prompt"` (non-interactive)
- `gh copilot` is just a thin passthrough wrapper that downloads/runs the standalone CLI — the real command is `copilot`, not `gh copilot suggest`

**Prevention:**  
- Verify every command example against `https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference`
- All interactive examples must start with `copilot` (not `gh copilot suggest/explain`)
- Programmatic examples: `copilot -p "..." --allow-tool='shell(git)'`

**Warning sign:**  
Any code block containing `gh copilot suggest` or `gh copilot explain`

**Phase:** CLI Intro section (Phase 1 / content authoring)

---

### Pitfall 2: Wrong Agent File Format — Conflating CLI Agents with Cloud Agents

**What goes wrong:**  
The guide shows agent example files as `NAME.md` (e.g., `.github/agents/security-expert.md`) when the correct format for **Copilot CLI custom agents** uses the `.agent.md` double extension (e.g., `.github/agents/security-expert.agent.md`).

**Why it happens:**  
There are two distinct agent systems in the GitHub Copilot ecosystem that share the `.github/agents/` directory:

| System | File format | Invocation |
|--------|-------------|------------|
| **Copilot CLI custom agents** | `.github/agents/NAME.agent.md` | `copilot --agent NAME` or `/agent` in CLI |
| **Copilot cloud agent** (browser-based) | `.github/agents/NAME.md` | GitHub Issues / cloud runner |

Both live in `.github/agents/`, but have different file extensions. The cloud agent format (`.md`) is what appears in most general Copilot documentation and is what an AI will generate by default.

**Consequences:**  
The CLI cannot resolve agents without the `.agent.md` extension. The "orchestration" example in the guide won't work when users try to replicate it. Programmtic invocation `copilot --agent NAME` depends on the filename (without the `.agent.md` extension).

**Correct CLI agent profile frontmatter:**
```yaml
---
name: security-expert          # display name (optional; filename used if omitted)
description: Reviews code for security vulnerabilities. Use when a security audit is requested.
# tools: [shell, write]        # optional: restrict tool access
---

You are a security specialist...
```

**Correct file path:** `.github/agents/security-expert.agent.md`

**Prevention:**  
- Always use `.agent.md` extension for CLI agent examples
- Add an explicit callout in the guide: "Note: Copilot CLI agents use `.agent.md`. Cloud agent files use `.md`. Both can exist in `.github/agents/`."

**Warning sign:**  
Any agent file example that ends in just `.md` in the context of CLI agent documentation.

**Phase:** Agents section (Phase 2 / content authoring)

---

### Pitfall 3: Wrong Skill Structure — `*.md` Files Instead of Skill Directories

**What goes wrong:**  
The guide shows skills as flat `.md` files (`.github/skills/webapp-testing.md`) when the correct structure is a **named directory containing a file called `SKILL.md`**:
```
.github/skills/webapp-testing/SKILL.md
                               convert-svg-to-png.sh   (optional scripts)
```

**Why it happens:**  
The natural assumption is that skills are just Markdown files, like custom instructions. The directory-based structure (each skill in its own subdirectory) is non-obvious and specific to the Agent Skills open standard specification.

**Consequences:**  
- Copilot CLI cannot discover skills not in the expected directory structure  
- `/skills list` will not show the skill  
- The example file path in the guide is unreproducible

**Critical rules from official docs:**
- The file inside the skill directory **must** be named `SKILL.md` (capital letters, exact name)
- The directory name should be lowercase with hyphens: `.github/skills/github-actions-failure-debugging/`
- Directory name typically matches the `name` frontmatter value
- Supported locations: `.github/skills/`, `.claude/skills/`, `.agents/skills/` (project); `~/.copilot/skills/` (personal)

**Correct SKILL.md frontmatter:**
```yaml
---
name: webapp-testing           # required; lowercase, hyphens for spaces
description: Automated testing guide for web applications. Use when asked to write or fix tests.
license: MIT                   # optional
allowed-tools: shell           # optional; SECURITY RISK — see Pitfall 4
---
```

**Prevention:**  
- Always show skill examples as directory trees, not flat files
- Add explicit note: "The file must be named `SKILL.md` — not `skill.md` or `webapp-testing.md`"

**Warning sign:**  
Any skills section showing `.github/skills/*.md` without the subdirectory structure.

**Phase:** Skills section (Phase 2 / content authoring)

---

### Pitfall 4: Missing `allowed-tools: shell` Security Warning in Skill Examples

**What goes wrong:**  
The guide includes `allowed-tools: shell` in a `SKILL.md` frontmatter example without the accompanying security warning. Users copy the pattern for their own skills without understanding the risk.

**Why it happens:**  
The `allowed-tools` field makes skill examples more "complete" and removes interactive confirmation prompts — so it appears in many examples. The security implication is buried.

**Consequences:**  
Users create skills with `allowed-tools: shell` from untrusted sources. The official docs warn: *"Only pre-approve the shell or bash tools if you have reviewed this skill and any referenced scripts, and you fully trust their source. Pre-approving shell or bash removes the confirmation step for running terminal commands and can allow attacker-controlled skills or prompt injections to execute arbitrary commands in your environment."*

**Prevention:**  
- Every code example with `allowed-tools: shell` must include an adjacent security callout
- Keep the example skill's `allowed-tools` field commented out by default; show how to enable it with the warning inline

**Warning sign:**  
`allowed-tools: shell` or `allowed-tools: bash` in any skill example without a warning callout.

**Phase:** Skills section (Phase 2 / content authoring)

---

### Pitfall 5: Confusing `gh agent-task` with Copilot CLI Agents

**What goes wrong:**  
Guide references `gh agent-task` as part of the "agents" section, implying it is how you create or run Copilot CLI custom agents.

**Why it happens:**  
`gh agent-task` appears in the `gh` CLI manual alongside all other `gh` subcommands. It has "agent" in its name. It feels related.

**Consequences:**  
`gh agent-task create "Improve performance"` creates a **GitHub Issue assigned to Copilot** — it's the cloud-side agentic task system, not a way to run CLI agents locally. The two are completely different:

| `gh agent-task create "..."` | `copilot --agent NAME -p "..."` |
|---|---|
| Creates a GitHub Issue on github.com | Runs a local CLI agent session |
| Requires org/enterprise feature access | Works with any Copilot subscription |
| Cloud agent processes the task | Local CLI agent processes the task |
| Also in preview | Also in preview (as of research date) |

**Prevention:**  
Do not conflate `gh agent-task` with CLI agent creation or invocation.

**Warning sign:**  
`gh agent-task` appearing in any section that claims to be about creating or running custom CLI agents.

**Phase:** Agents section (Phase 2 / content authoring)

---

## Moderate Pitfalls

---

### Pitfall 6: Stale Commands in an Actively Evolving Product

**What goes wrong:**  
The guide ships with specific command syntax, slash command names, or flag names that are later renamed, removed, or changed. The official docs themselves note: *"GitHub Copilot CLI is continually evolving. Use the `/help` command to see the most up to date information."*

**Why it happens:**  
The CLI is in active development. Many features carry a "preview" label. The product ships breaking changes to slash commands and flags.

**Known stability risks as of research:**
- `gh agent-task` — explicitly marked "in preview and subject to change without notice"
- `gh copilot` wrapper — "currently in preview and subject to change"
- `/fleet`, `/delegate`, `/on-air` — experimental/newer commands with less stable interfaces
- Built-in agent names (`explore`, `task`, `general-purpose`, `code-review`, `research`) are stable but their behavior can change

**Prevention:**  
- Mark any command with a "📌 Verify this is current" comment during development
- Avoid documenting very-new experimental features if stability is a goal
- In the guide itself, add a "Last verified" date and a note pointing to `copilot help` for current commands
- Focus on slash commands that appear in the stable reference (`/plan`, `/agent`, `/skills`, `/model`, `/review`, `/research`)

**Warning sign:**  
Any command described as "new" or "experimental" during authoring, or any flag without documentation in the official CLI command reference.

**Phase:** All content sections; mitigated by adding a "last verified" date in the guide's header.

---

### Pitfall 7: Missing Custom Instructions File Discovery Order

**What goes wrong:**  
Guide only shows `.github/copilot-instructions.md` as the custom instructions file. Users are confused when global personal instructions don't take effect, or when `AGENTS.md` is mentioned elsewhere and seems to do the same thing.

**Why it happens:**  
The CLI actually reads from multiple instruction files, with a discovery order:

| Location | Scope |
|----------|-------|
| `~/.copilot/copilot-instructions.md` | All sessions (global) |
| `.github/copilot-instructions.md` | Repository |
| `.github/instructions/**/*.instructions.md` | Repository (modular) |
| `AGENTS.md` (in Git root or cwd) | Repository |
| `Copilot.md`, `GEMINI.md`, `CODEX.md` | Repository |

Repository instructions take precedence over global. This is a frequently misunderstood area.

**Prevention:**  
Include the full discovery table in the Custom Instructions subsection. Clarify that `AGENTS.md` is a cross-tool convention (also recognized by Claude, Gemini agents) not specific to GitHub Copilot.

**Phase:** Content authoring, CLI intro/customization section.

---

### Pitfall 8: Single-File HTML — External CDN Dependencies Breaking Offline Use

**What goes wrong:**  
The HTML file links to a CDN (e.g., `<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/...">` or Google Fonts) — working fine when online, but completely broken when opened from a local file without internet access.

**Why it happens:**  
CDN links are the easiest way to add syntax highlighting (highlight.js, Prism.js) and icon sets. The requirement of being "self-contained" is easy to forget while adding features.

**Consequences:**  
Code blocks render as plain unstyled text. Icons disappear. The terminal dark theme loses all typographic polish if fonts are loaded from Google Fonts.

**Prevention:**  
- Use system font stacks only: `font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace`
- Inline all CSS in `<style>` tags
- Inline all JS in `<script>` tags
- Embed syntax highlighting library (e.g., a minified subset of highlight.js) directly in the `<script>` block, or implement CSS-only token coloring for the specific languages needed (YAML, shell, Markdown)
- Run a "disconnect from internet, open file" smoke test before shipping

**Warning sign:**  
Any `<link>`, `<script src="...">`, or CSS `@import url(...)` pointing to an external domain.

**Phase:** Implementation (Phase 1 / technical build).

---

### Pitfall 9: Missing Copy Buttons on Code Blocks

**What goes wrong:**  
Code examples for CLI commands, SKILL.md frontmatter, and agent profile templates are shown in `<pre><code>` blocks with no copy button. Users manually select text, especially on multi-line YAML, where misselection causes formatting errors.

**Why it happens:**  
Copy buttons are a JavaScript feature that requires extra implementation in a single-file HTML context. Easy to defer and forget.

**Consequences:**  
Friction on the most used part of the guide. Users skip examples. Errors from manual copy-paste (leading spaces in YAML are critical).

**Prevention:**  
Implement a minimal copy button with `navigator.clipboard.writeText()`. A ~10 line vanilla JS function covers this completely. Style it to appear on hover over code blocks. This is table-stakes for developer documentation.

**Warning sign:**  
No `navigator.clipboard` usage in the `<script>` block.

**Phase:** Implementation (Phase 1 / technical build).

---

### Pitfall 10: Flat Skill File Examples Omitting the Directory Context

**What goes wrong:**  
The guide shows a SKILL.md snippet without showing the directory structure that contains it, so users don't know where to put it or that the enclosing directory name matters.

**Why it happens:**  
It's easier to show a code block with the YAML frontmatter than to also show a file tree diagram. The tree gets cut when formatting examples.

**Prevention:**  
Every skill example must open with a directory tree before showing the file content:

```
.github/
└── skills/
    └── github-actions-failure-debugging/  ← directory name = skill name
        ├── SKILL.md                        ← must be named exactly SKILL.md
        └── debug-helper.sh                 ← optional scripts
```

Then show the `SKILL.md` content block.

**Phase:** Skills section (Phase 2 / content authoring).

---

## Minor Pitfalls

---

### Pitfall 11: Tabs/Collapsibles Breaking Without JS — Pure CSS Approach Has Limits

**What goes wrong:**  
CSS-only `<details>/<summary>` collapsibles and radio-button tab tricks work but have quirks:
- `<details>` cannot be forced open/closed via URL hash
- Radio-button tabs don't persist state across section navigation
- No keyboard accessibility for custom CSS tab implementations

**Prevention:**  
Use `<details>/<summary>` for collapsibles (native HTML, works everywhere). For tabs, use minimal vanilla JS (~30 lines) with `aria-selected` and `role="tabpanel"` for accessibility. Store tab state in `sessionStorage` if persistence matters.

**Phase:** Implementation (Phase 1 / technical build).

---

### Pitfall 12: Missing `<meta charset="utf-8">` and Viewport Tag

**What goes wrong:**  
YAML frontmatter examples contain characters that render as mojibake on some systems without charset declaration. Mobile viewport isn't set, making the guide unusable on phones/tablets even though this is "just for sharing."

**Prevention:**  
```html
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
```

These two lines are mandatory for any HTML file. Non-negotiable.

**Phase:** Implementation (Phase 1 / technical build).

---

### Pitfall 13: No Anchor Links on Section Headings

**What goes wrong:**  
A guide this long (CLI intro + agents + skills + orchestration) needs deep-linkable sections. Without anchor links, users cannot share "check out the agents section" as a URL.

**Prevention:**  
Add `id` attributes to every `<h2>` and `<h3>`. Add a `#` anchor link that appears on heading hover. Implement a sticky table of contents or in-page nav with anchor links.

**Phase:** Implementation (Phase 1 / technical build).

---

### Pitfall 14: Frontmatter Field Name Typos in Examples

**What goes wrong:**  
The guide uses `title:` instead of `name:` in SKILL.md frontmatter, or `prompt:` instead of the Markdown body (there is no `prompt:` frontmatter field for CLI skills), or `tools:` instead of `allowed-tools:`.

**Why it happens:**  
These field names are non-standard and easy to misremember. Agent profiles use `description:` and have a Markdown body for the prompt — there is no `prompt:` YAML key for skills.

**Correct fields for SKILL.md:**
| Field | Required | Notes |
|-------|----------|-------|
| `name` | ✅ Yes | lowercase, hyphens |
| `description` | ✅ Yes | triggers when Copilot selects the skill |
| `license` | No | free text |
| `allowed-tools` | No | `shell`, `bash` — with security warning |

**Correct fields for agent `.agent.md`:**
| Field | Required | Notes |
|-------|----------|-------|
| `name` | No | display name; filename used if omitted |
| `description` | No | but strongly recommended for triggering |
| `tools` | No | restrict tool access |
| `mcp-servers` | No | MCP server configs |

The Markdown body (below the `---` closing) is where agent instructions go — no `prompt:` YAML key exists.

**Warning sign:**  
`title:`, `prompt:`, `id:`, `version:`, `author:` in SKILL.md or agent frontmatter examples.

**Phase:** Content authoring, all sections with code examples.

---

## Phase-Specific Warnings

| Phase Topic | Likely Pitfall | Mitigation |
|-------------|----------------|------------|
| CLI Intro section | Wrong CLI identity (old `gh copilot suggest`) | Verify every command against CLI command reference |
| CLI Intro — installation | Missing Node 22+ requirement for npm install | Call out prerequisite explicitly |
| Agents section — file format | `.md` instead of `.agent.md` for CLI agent files | Use `.agent.md` in all CLI agent examples |
| Agents section — terminology | `gh agent-task` conflated with CLI agents | Explicitly distinguish in a callout box |
| Skills section — structure | Flat `*.md` file instead of directory + `SKILL.md` | Always show directory tree before file content |
| Skills section — security | `allowed-tools: shell` without warning | Add warning callout adjacent to every such example |
| Skills section — file naming | `skill.md` (lowercase) instead of `SKILL.md` | Bold the exact filename in the guide |
| HTML build — self-contained | External CDN links breaking offline | Pre-flight: open file with network disabled |
| HTML build — code blocks | No copy button | Implement copy button in Phase 1 build |
| Custom instructions | Only showing `.github/copilot-instructions.md` | Show full discovery-order table |
| Any section with "new" features | `gh agent-task`, `/fleet`, `/delegate` — all in preview | Add "preview — may change" notice |

---

## Sources

- `https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/create-skills` — Skills format, directory structure, SKILL.md frontmatter (HIGH confidence — official docs)
- `https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli` — CLI agent format, `.agent.md` extension (HIGH confidence — official docs)
- `https://docs.github.com/en/copilot/concepts/agents/about-agent-skills` — Agent skills open standard, supported directories (HIGH confidence — official docs)
- `https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-custom-agents` — Cloud agent vs CLI agent distinction, `.github/agents/` format (HIGH confidence — official docs)
- `https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference` — All slash commands, keyboard shortcuts, CLI flags (HIGH confidence — official reference)
- `https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-config-dir-reference` — `~/.copilot/` directory structure (HIGH confidence — official reference)
- `https://docs.github.com/en/copilot/how-tos/copilot-cli/cli-getting-started` — Installation commands, `copilot` (not `gh copilot suggest`) (HIGH confidence — official docs)
- `https://docs.github.com/en/copilot/how-tos/copilot-cli/cli-best-practices` — Custom instructions discovery order, tool permissions, models (HIGH confidence — official docs)
- `https://cli.github.com/manual/gh_agent-task` — `gh agent-task` as separate feature for cloud-side task assignment (HIGH confidence — official gh CLI manual)
- `https://cli.github.com/manual/gh_copilot` — `gh copilot` as thin wrapper (HIGH confidence — official gh CLI manual)

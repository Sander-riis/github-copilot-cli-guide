# Feature Landscape: GitHub Copilot CLI Agents & Skills Guide

**Domain:** Developer reference guide — GitHub Copilot CLI (agents, skills, orchestration)
**Researched:** 2025-01-10
**Sources:** Official GitHub Docs (docs.github.com) — HIGH confidence
**Key URLs verified:**
- https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-custom-agents
- https://docs.github.com/en/copilot/concepts/agents/about-agent-skills
- https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
- https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/create-skills
- https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference
- https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli-agents/invoke-custom-agents
- https://docs.github.com/en/copilot/concepts/agents/copilot-cli/fleet

---

## Table Stakes

Content users expect when they open a "Copilot CLI agents & skills guide." Missing any of these makes the page feel incomplete.

| Feature | Why Expected | Complexity | Notes |
|---------|--------------|------------|-------|
| Core CLI commands cheat sheet | First thing experienced users reach for | Low | `copilot`, `-p`, `-s`, `--agent`, `--model`, `--allow-all-tools`, `--autopilot`, etc. |
| Interactive slash commands reference | Slash commands are the primary interface | Medium | 30+ commands; curate the high-value ones (`/agent`, `/fleet`, `/plan`, `/skills`, `/model`, `/review`, `/delegate`, `/research`) |
| Keyboard shortcut table | Shortcut mastery separates power users | Low | `Shift+Tab` mode cycling, `@FILENAME`, `!CMD`, `Ctrl+C`, `Esc`, `Ctrl+L` |
| Agent profile format with real example | Core deliverable of this guide | Medium | `.agent.md` extension, YAML frontmatter (`name`, `description`, `prompt`/body, `tools`, `mcp-servers`) |
| Agent file locations table | Users need to know WHERE to put agents | Low | `.github/agents/` (repo), `~/.copilot/agents/` (user), `/agents/` in `.github-private` (org/enterprise) |
| Built-in agents inventory | Without this, users don't know what's already available | Low | `explore`, `task`, `general-purpose`, `code-review`, `research` — each with distinct constraints |
| Skill format with real example | Core deliverable for the skills section | Medium | `SKILL.md` file (exact name required), YAML frontmatter (`name`, `description`, `license`, `allowed-tools`), skill directory structure |
| Skill file locations table | Users need to know WHERE to put skills | Low | `.github/skills/`, `.claude/skills/`, `.agents/skills/` (project); `~/.copilot/skills/` (personal) |
| Skills CLI commands | How to manage skills at runtime | Low | `/skills list`, `/skills info`, `/skills add`, `/skills reload`, `/skills remove SKILL-DIRECTORY` |
| Subagent/orchestration explanation | Why agents run in separate context windows | Medium | Main agent = orchestrator; subagents = separate context; why this matters for large tasks |
| `/fleet` command explanation | Flagship parallel execution feature | Medium | Main agent analyzes task, decomposes, runs subagents in parallel where dependencies allow |
| Agent invocation methods | Users need multiple paths to invoke agents | Low | `/agent` command, prompt mention, trigger-word inference, `--agent` flag |
| Skills vs custom instructions distinction | Common confusion point | Low | Custom instructions = always-on simple context; skills = loaded on-demand when relevant |

---

## Differentiators

Content that transforms a basic reference into an excellent guide. Not expected, but high value.

| Feature | Value Proposition | Complexity | Notes |
|---------|-------------------|------------|-------|
| **Complete annotated agent example file** | Readers can copy-paste and immediately use | Low | Full `.agent.md` with frontmatter + body; explain each field inline |
| **Complete annotated SKILL.md example file** | Same copy-paste value for skills | Low | Full `SKILL.md` with frontmatter, body instructions, and a linked script example |
| **Script-enabled skill example** | Shows the power beyond pure instructions | Medium | SKILL.md + companion shell script in skill directory; `allowed-tools: shell` with security warning |
| **Built-in agents cheat sheet** | Saves "which agent does what?" research time | Low | Side-by-side: `explore` (read-only, parallelizable), `task` (commands/builds), `code-review` (no style, no file changes), `research` (`/research` only) |
| **Mode cycling visual** (`Shift+Tab`) | Many users don't know standard → plan → autopilot exists | Low | Simple diagram or annotated explanation of the three modes |
| **Fleet + autopilot workflow walkthrough** | Power pattern that unlocks autonomous multi-step work | Medium | Step-by-step: plan mode → review plan → accept + autopilot + /fleet |
| **Naming conflict resolution rules** | Prevents confusion when agents exist at multiple levels | Low | User-level overrides repo-level; repo-level overrides org-level |
| **`/research` special invocation note** | Research agent is the one built-in that CAN'T be auto-triggered | Low | Only invoked explicitly via `/research`; contrast with other built-ins |
| **`--agent` programmatic flag** | Enables CI/scripting use cases | Low | `copilot --agent security-auditor --prompt "..."` — filename without `.agent.md` extension |
| **Tool permission patterns** | Security-aware users want to understand the approval system | Medium | `--allow-all-tools` vs `--allow-tool` vs `--deny-tool`; `/yolo`; `allowed-tools` in SKILL.md frontmatter |
| **Agent Skills open standard note** | Explains cross-tool compatibility (VS Code, cloud agent) | Low | Agent Skills spec used by multiple AI systems; skills work with Copilot CLI, cloud agent, VS Code agent mode |
| **Community skills resources** | Points users toward reusable skill libraries | Low | `anthropics/skills`, `github/awesome-copilot` collection |
| **`@FILENAME` context inclusion** | Power user shortcut that's underused | Low | Include files in context mid-prompt; mention alongside `!COMMAND` shell passthrough |
| **Session management** | Underdiscovered feature for long-running work | Medium | `/session`, `--resume`, `--continue`, `/share` to gist/file |
| **Custom model provider (BYOK)** | Unlocks Ollama, Azure OpenAI, Anthropic direct | Medium | `COPILOT_PROVIDER_BASE_URL`, `COPILOT_PROVIDER_TYPE`, `COPILOT_PROVIDER_API_KEY`, `COPILOT_MODEL` env vars |
| **`/fleet` with `@AGENT-NAME` syntax** | How to direct fleet subtasks to specific custom agents | Low | Within a fleet prompt: `Use @test-writer to create comprehensive unit tests for ...` |

---

## Anti-Features

Content to deliberately NOT include — it dilutes focus or targets the wrong audience.

| Anti-Feature | Why Avoid | What to Do Instead |
|--------------|-----------|-------------------|
| Installation/setup walkthrough | Out of scope — guide assumes existing Copilot CLI users | One-line reference: "Install with `npm install -g @github/copilot`" at most |
| Authentication flow | Beginner content; guide assumes already authenticated | Skip entirely or note in one sentence |
| GitHub Copilot Chat/IDE features | Explicitly out of scope per PROJECT.md | Hard boundary: note "this guide covers CLI only" if confusion likely |
| Copilot cloud agent management (GitHub.com UI) | Different product surface | Brief note that agent skills also work with cloud agent, link out |
| MCP server creation/building | Deep rabbit hole; entire separate guide | Cover `/mcp add` command, mention GitHub MCP server is built-in, stop there |
| Plugin API creation | Developer audience but wrong layer | List `/plugin` command, note plugin marketplace exists, link out |
| Billing/quota details | Administrative, not developer workflow | Mention premium request multiplier in one sentence where relevant |
| Org/enterprise policy configuration | Admin task, not developer workflow | Mention org-level agent location; skip policy enforcement details |
| Copilot Memory feature | Tangential; dedicated concept docs | Skip or mention as a "further reading" link |
| Explaining basic shell/git concepts | Wrong audience assumption | Trust the reader |

---

## Feature Dependencies

```
CLI Commands (interactive + flags) → must exist before any examples make sense

Agent Profile Format
  → Agent file locations
  → Built-in agents inventory
  → Agent invocation methods
  → Subagent/orchestration explanation
      → /fleet command
          → Fleet + autopilot workflow

Skill Format (SKILL.md)
  → Skill locations
  → Skills CLI commands
  → Script-enabled skill example
  → Skills vs custom instructions distinction
  → allowed-tools security note
```

---

## Content Structure Recommendation

Based on the three primary content segments in PROJECT.md (CLI Intro → Agents → Skills):

### Section 1: CLI Intro (narrative, context-setting)
Covers key commands and concepts that experienced users need to know. Not a full reference dump — curated and narrative.

**Must include:**
- `copilot` / `copilot -p "..."` / `copilot -sp "..."` patterns
- Mode cycling: standard → plan → autopilot (`Shift+Tab`)
- Key shortcuts: `@FILENAME`, `!COMMAND`, `Esc`, `/help`
- Top 8–10 slash commands (`/plan`, `/fleet`, `/model`, `/review`, `/delegate`, `/research`, `/diff`, `/pr`)
- Command-line flags for power users (`--allow-all-tools`, `--autopilot`, `--agent`, `--resume`)

**Differentiating:**
- Modes diagram (standard / plan / autopilot)
- The `/research` special-status note
- Session resume workflow

### Section 2: Agents (with annotated example)
Covers what agents are, all ways to define and invoke them, built-in agents, and orchestration.

**Must include:**
- What agents are vs regular Copilot prompts
- Agent profile format (full annotated `.agent.md` example)
- Agent file locations table (repo vs user vs org)
- Built-in agents table (all five with constraints noted)
- Subagent concept: separate context window, why it matters
- Agent invocation methods (all four: `/agent`, mention, inference, `--agent`)
- `/fleet` orchestration: how it works, when to use it

**Differentiating:**
- Naming conflict resolution (user > repo > org)
- `/fleet` + autopilot walkthrough
- `@AGENT-NAME` syntax inside fleet prompts
- Full copy-paste agent profile example

### Section 3: Skills (with annotated example)
Covers what skills are, how they differ from agents and custom instructions, full creation workflow.

**Must include:**
- What skills are vs agents vs custom instructions
- Skills as an open standard (cross-tool compatibility)
- `SKILL.md` format (full annotated example)
- Skill directory structure (subdirectory, `SKILL.md`, optional scripts)
- Skill file locations (project vs personal)
- Skills CLI commands (`/skills list`, `info`, `add`, `reload`, `remove`)
- Invoking a skill: `/skill-name` in a prompt

**Differentiating:**
- Script-enabled skill (SKILL.md + `convert-svg-to-png.sh` pattern)
- `allowed-tools` field with security warning about `shell`/`bash`
- Community resources (`anthropics/skills`, `github/awesome-copilot`)
- Practical distinction: when to use skills vs agents vs custom instructions

---

## Accurate Technical Details (verified from official docs)

### Agent Profile Frontmatter Fields
```yaml
---
name: readme-creator          # optional; filename used as ID if omitted
description: "Agent purpose"  # required; tells Copilot WHEN to invoke
                               # and what expertise it provides
tools:                         # optional; default=all available
  - view
  - shell
mcp-servers:                   # optional; MCP server configs
  - ...
---
Instructions for the agent go in the Markdown body (acts as system prompt)
```

### Agent File Naming
- Extension: `.agent.md` (not just `.md`)
- Example: `security-expert.agent.md`
- Programmatic reference uses filename without extension: `--agent security-expert`

### SKILL.md Frontmatter Fields
```yaml
---
name: github-actions-failure-debugging  # required; lowercase, hyphens for spaces
description: "What skill does, when to use it"  # required; drives auto-invocation
license: MIT                             # optional
allowed-tools: shell                     # optional; pre-approves tool use
                                         # WARNING: shell/bash removes confirmation step
---
Instructions, steps, and guidance in the Markdown body.
Reference companion scripts by relative path.
```

### Skill Directory Structure
```
.github/skills/
└── webapp-testing/           # subdirectory per skill (lowercase, hyphens)
    ├── SKILL.md              # required; must be named SKILL.md exactly
    └── run-tests.sh          # optional; referenced from SKILL.md body
```

### Built-in Agents (authoritative list)
| Agent | Invoked By | Can Run in Parallel | Changes Files | Notes |
|-------|-----------|---------------------|---------------|-------|
| `explore` | Auto-inference or subagent | ✅ Yes (read-only) | ❌ No | Fast codebase search; read-only GitHub MCP access |
| `task` | Auto-inference or subagent | Context-dependent | Via commands | Runs builds/tests/linters; brief success, full failure output |
| `general-purpose` | Auto-inference or subagent | ✅ When appropriate | ✅ Yes | Full capabilities; separate context; same tools as main agent |
| `code-review` | Auto-inference or subagent | ✅ Yes | ❌ No | Bugs/security/race conditions/memory leaks only; never style |
| `research` | `/research` ONLY | ❌ No | ❌ No | Only explicit invocation; web fetch/search; never auto-triggered |

### Modes (Shift+Tab cycles through)
1. **Standard** — Normal interactive
2. **Plan** — Creates implementation plan first; user reviews before execution
3. **Autopilot** — Works autonomously; auto-responds to tool confirmation requests

---

## MVP Recommendation

Build the guide in strict content priority order:

**Must ship to be useful:**
1. CLI commands section (curated, narrative — not an exhaustive dump)
2. Full annotated agent profile example with field-by-field explanation
3. Built-in agents table with invocation notes
4. Full annotated SKILL.md example with directory structure
5. Skills CLI commands (`/skills list`, `info`, `reload`, etc.)

**Elevates to "excellent":**
6. Subagent/orchestration explanation (why separate context window matters)
7. `/fleet` walkthrough with the autopilot integration pattern
8. Script-enabled skill example (SKILL.md + companion script)
9. Mode diagram (standard / plan / autopilot with Shift+Tab)
10. Skill invocation: `/skill-name` syntax in a prompt

**Defer if time-constrained:**
- Custom model provider (BYOK env vars) — niche but valuable; add as collapsible
- Session management deep dive — useful but secondary to agents/skills
- Plugin system — out of scope for this guide's core audience

---

## Sources

All content verified against official GitHub Copilot documentation (docs.github.com, 2025):

| Source | Confidence | Content Covered |
|--------|-----------|-----------------|
| [About custom agents (CLI)](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-custom-agents) | HIGH | Agent profile format, frontmatter, file locations, built-in agents |
| [About agent skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills) | HIGH | What skills are, locations, open standard |
| [Create custom agents for CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli) | HIGH | `.agent.md` format, AI-assisted creation, invocation methods |
| [Create agent skills](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/create-skills) | HIGH | SKILL.md format, directory structure, script integration, `allowed-tools` |
| [CLI command reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference) | HIGH | All slash commands, command-line options, keyboard shortcuts |
| [Invoke custom agents](https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli-agents/invoke-custom-agents) | HIGH | Invocation methods, built-in agent table |
| [Fleet concept](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/fleet) | HIGH | /fleet mechanics, orchestration, parallelization, autopilot integration |
| [CLI getting started](https://docs.github.com/en/copilot/how-tos/copilot-cli/cli-getting-started) | HIGH | Installation, basic commands, core shortcuts |
| [About Copilot CLI (concepts)](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-copilot-cli) | HIGH | Tool approval system, model config, BYOK env vars |

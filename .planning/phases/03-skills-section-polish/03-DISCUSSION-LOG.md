# Phase 3: Skills Section & Polish - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-04-10
**Phase:** 03-skills-section-polish
**Areas discussed:** Skills Overview structure, SKILL.md examples depth, Security warning tone

---

## Skills Overview Structure

| Option | Description | Selected |
|--------|-------------|----------|
| Directory tree first | Show .github/skills/my-skill/SKILL.md structure up front, then explain | ✓ |
| Concept explanation first | Explain what skills are first, then directory tree | |
| Disambiguation callout first | Mirror agents tab pattern with callout first | |

**User's choice:** Directory tree first
**Notes:** User wants the concrete file structure to lead

| Option | Description | Selected |
|--------|-------------|----------|
| Brief inline note | One sentence distinguishing skills from agents | ✓ |
| Dedicated callout box | callout--info block for skills vs agents vs custom-instructions | |
| Table comparison | 3-row comparison table | |

**User's choice:** Brief inline note

| Option | Description | Selected |
|--------|-------------|----------|
| Info callout for open standard | callout--info noting cross-platform support | ✓ |
| Inline paragraph | Weave into narrative | |
| You decide | Agent's discretion | |

**User's choice:** Info callout

| Option | Description | Selected |
|--------|-------------|----------|
| Collapsible file locations | 2-row table in collapsible | ✓ |
| Inline paragraph | Mention paths in text | |
| You decide | Agent's discretion | |

**User's choice:** Collapsible for file locations

| Option | Description | Selected |
|--------|-------------|----------|
| Mirror Agents tab split | Overview = all overview content, Reference = CLI commands + invocation | ✓ |
| Pack into overview | Reference = just CLI commands table | |
| You decide | Agent's discretion | |

**User's choice:** Mirror Agents tab split

| Option | Description | Selected |
|--------|-------------|----------|
| Tip callout in skills-reference | callout--tip at end of skills-reference | ✓ |
| Inline mention | Links in closing paragraph | |
| Separate Community subsection | h3 section at end of reference | |

**User's choice:** Tip callout at end of skills-reference

| Option | Description | Selected |
|--------|-------------|----------|
| Header with last-verified date | Muted text in page header | ✓ |
| Footer at bottom | End of page | |
| Meta tag only | Not visible | |
| Skip | Not important | |

**User's choice:** Header with last-verified date

---

## SKILL.md Examples Depth

| Option | Description | Selected |
|--------|-------------|----------|
| Fabricated but realistic | Plausible skill showcasing all fields | ✓ |
| Real GSD skill | Use existing ~/.copilot/skills/ skill | |
| Both | One fabricated, one real | |

**User's choice:** Fabricated but realistic

| Option | Description | Selected |
|--------|-------------|----------|
| Filepath header + inline YAML comments | Same pattern as .agent.md example | ✓ |
| Separate field reference table | Code block then table below | |
| Both | Annotated code block + collapsible table | |

**User's choice:** Filepath header + inline YAML comments

| Option | Description | Selected |
|--------|-------------|----------|
| Side-by-side in one collapsible | SKILL.md + .sh file + danger callout in collapsible | ✓ |
| Separate section | Its own h3 subsection | |
| Sequential code blocks | Two blocks, no collapsible | |

**User's choice:** Side-by-side in one collapsible

| Option | Description | Selected |
|--------|-------------|----------|
| Collapsible field reference | Table with name, description, license, allowed-tools | ✓ |
| No separate reference | Inline comments sufficient | |
| You decide | Agent's discretion | |

**User's choice:** Collapsible field reference

| Option | Description | Selected |
|--------|-------------|----------|
| Collapsible CLI commands table | Table in skills-reference | ✓ |
| Flat table | Always visible | |
| Code block examples | Each command as bash block | |

**User's choice:** Collapsible CLI commands table

---

## Security Warning Tone

| Option | Description | Selected |
|--------|-------------|----------|
| Strong danger callout | callout--danger with clear warning about arbitrary commands | ✓ |
| Official docs verbatim | Exact language from GitHub docs | |
| Moderate warning | callout--warning (yellow) with balanced note | |

**User's choice:** Strong danger callout

| Option | Description | Selected |
|--------|-------------|----------|
| Inside script-enabled collapsible only | After shell script code block, contextually relevant | ✓ |
| Both inside and standalone | Reinforce warning in two places | |
| Standalone in overview only | Prominent but not repeated | |

**User's choice:** Inside the script-enabled collapsible only

---

## Agent's Discretion

- Exact wording and naming of fabricated skill examples
- Exact security warning phrasing (must be strong and clear)
- Narrative paragraph flow and tone

## Deferred Ideas

None — discussion stayed within phase scope

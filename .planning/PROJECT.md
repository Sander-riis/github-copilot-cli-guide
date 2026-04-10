# GitHub Copilot CLI — AI Agents & Skills Guide

## What This Is

A single-page interactive HTML reference guide for developers already using GitHub Copilot CLI who want to level up with agents and skills. It covers the CLI's essential commands through narrative walkthroughs, then dives into creating and orchestrating custom agents and skills — with real example files throughout.

## Core Value

Experienced Copilot CLI users can understand, create, and orchestrate agents and skills without hunting through scattered documentation.

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] Narrative CLI intro section with key commands explained in context
- [ ] Agent creation section covering both AI-assisted and manual workflows
- [ ] Orchestration subsection with a real `.github/agents/*.md` example file
- [ ] Skills section with a complete example `.github/skills/*.md` file
- [ ] Dark terminal-inspired visual theme
- [ ] Interactive layout with collapsible sections and tabs between topics
- [ ] Single self-contained HTML file (no external dependencies)

### Out of Scope

- Multi-page site — single file only, for easy sharing
- Backend or server — static HTML only
- Copilot Chat / IDE features — CLI focus only
- Beginner onboarding — assumes existing Copilot CLI familiarity

## Context

- Target users are developers already using Copilot CLI who want to unlock the agents/skills layer
- Page should feel like polished internal documentation or a DevRel resource
- All examples should use real Copilot CLI file formats and frontmatter
- Three primary content segments: CLI Intro → Agents → Skills

## Constraints

- **Format**: Single `.html` file — must be fully self-contained
- **Style**: Dark theme, terminal-inspired — code-heavy sections should feel at home
- **Interactivity**: Collapsible sections + tabs; no heavy framework dependencies

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Single HTML file | Easy to share, no hosting needed | — Pending |
| Dark terminal theme | Matches CLI/developer aesthetic | — Pending |
| Tabs + collapsibles | Lets users explore non-linearly | — Pending |
| Full agent/skill examples | Concrete over abstract for this audience | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd-complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-04-10 after initialization*

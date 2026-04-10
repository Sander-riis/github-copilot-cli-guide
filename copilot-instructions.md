<!-- GSD:project-start source:PROJECT.md -->
## Project

**GitHub Copilot CLI — AI Agents & Skills Guide**

A single-page interactive HTML reference guide for developers already using GitHub Copilot CLI who want to level up with agents and skills. It covers the CLI's essential commands through narrative walkthroughs, then dives into creating and orchestrating custom agents and skills — with real example files throughout.

**Core Value:** Experienced Copilot CLI users can understand, create, and orchestrate agents and skills without hunting through scattered documentation.

### Constraints

- **Format**: Single `.html` file — must be fully self-contained
- **Style**: Dark theme, terminal-inspired — code-heavy sections should feel at home
- **Interactivity**: Collapsible sections + tabs; no heavy framework dependencies
<!-- GSD:project-end -->

<!-- GSD:stack-start source:research/STACK.md -->
## Technology Stack

## Recommended Stack
### Syntax Highlighting
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| highlight.js (core) | 11.11.1 | Syntax highlighting engine | Core-only build is 22.7KB; far leaner than full 125KB bundle |
| hljs `bash` language | 11.11.1 | Highlight CLI commands | Primary language in this guide |
| hljs `yaml` language | 11.11.1 | Highlight agent/skill frontmatter | YAML is the agent/skill file format |
| hljs `json` language | 11.11.1 | Highlight config snippets | Lightweight (0.7KB) |
| hljs `markdown` language | 11.11.1 | Highlight `.md` agent/skill files | 2.4KB, shows full file examples |
| hljs `shell` language | 11.11.1 | Distinguish shell sessions ($ prompt) | 0.6KB complement to bash |
| `github-dark` theme | 11.11.1 | Code block color scheme | GitHub's own dark palette — exactly on-brand for Copilot CLI |
- The `github-dark` theme precisely matches GitHub's dark UI (background `#0d1117`, text `#c9d1d9`) — ideal for a GitHub Copilot guide
- Core + languages path on jsdelivr is clean: `lib/core.min.js`, `lib/languages/bash.min.js`, etc.
- Prism v1.30.0 (March 2025) is equally valid but lacks a native GitHub-branded theme; budget difference is negligible (~30KB vs 33KB)
### Interactivity
| Approach | Library/API | Why |
|----------|-------------|-----|
| Tabs | Vanilla JS (~30 lines) | No library needed; `data-target` attributes + CSS class toggling covers the full requirement |
| Collapsibles | Native `<details>/<summary>` + CSS | Zero JS; browser-native; animatable with `max-height` transition; `::marker` is customizable |
| Copy to clipboard | `Navigator.clipboard.writeText()` | Browser-native since 2018; no library; one function on each `<pre>` block |
### Styling
| Approach | Why |
|----------|-----|
| CSS Custom Properties (variables) | One-place theming — change `--bg-page` and everything updates; no preprocessor needed |
| No CSS framework | Bootstrap minified CSS is ~100KB; Tailwind with JIT is still 30–80KB purged. Both outweigh the entire rest of the page. Pure custom CSS is 5–15KB and purpose-built. |
| System font stack (UI) | No font download; renders crisply on every OS; immediately available |
| Monospace font stack (code) | Cascading to OS-native mono fonts avoids a 30–100KB web font download |
### Self-Containment Strategy
## Color Palette: GitHub Dark Terminal Theme
| Token | Hex | Usage |
|-------|-----|-------|
| `--bg-page` | `#0d1117` | Full-page background; matches GitHub.com dark mode |
| `--bg-surface` | `#161b22` | Cards, code block backgrounds, tab panels |
| `--bg-elevated` | `#21262d` | Hover states, active tab background |
| `--text-primary` | `#c9d1d9` | All body text |
| `--text-muted` | `#8b949e` | Labels, captions, `<summary>` secondary text |
| `--text-link` | `#58a6ff` | Links, active tab underline accent |
| `--accent-green` | `#3fb950` | CLI success output, inline `$` prompt indicators |
| `--accent-purple` | `#d2a8ff` | Section headings, function names in code |
| `--accent-orange` | `#ffa657` | Built-ins, warnings |
| `--border` | `#30363d` | All card/tab/section dividers |
| `--border-muted` | `#21262d` | Subtle separators |
## Typography
## HTML Patterns
### Tabs
### Collapsibles
### Code Blocks with Copy Button
### highlight.js Initialization
## Alternatives Considered
| Category | Recommended | Alternative | Why Not |
|----------|-------------|-------------|---------|
| Syntax highlighting | highlight.js 11.11.1 (core + langs) | Prism.js 1.30.0 | Both equivalent; hljs `github-dark` theme is on-brand for this project |
| Syntax highlighting | highlight.js (inline, ~33KB) | Full highlight.min.js bundle | Full bundle is 125KB — 4× larger with 180+ unused languages |
| Collapsibles | `<details>/<summary>` (native) | JS accordion library | Native HTML5 needs zero JS, works without script, accessible by default |
| Copy button | `navigator.clipboard` (native) | `highlightjs-copy` plugin (1.7KB) | Clipboard API is available in all modern browsers; keeping the logic visible in source is cleaner |
| CSS theming | CSS custom properties | Sass/Less variables | No build step; custom properties work at runtime; just as powerful for a single theme |
| CSS framework | None (bespoke CSS) | Bootstrap 5, Tailwind | Bootstrap min CSS is ~100KB; even purged Tailwind is 30KB+; both outweigh everything else combined |
| JS framework | Vanilla JS | React, Vue, Svelte | Static content doesn't need component lifecycle, virtual DOM, or module bundling |
| Web fonts | System font stack | Google Fonts CDN | Eliminates external network request; no FOUT; privacy-safe; developer audience already has good monospace fonts |
| Build tooling | Optional Node build script | Vite, Webpack, Parcel | Full bundlers are engineering overhead for one HTML file; `fetch + fs.writeFileSync` covers the need in 30 lines |
| Icons | Inline SVG | Font Awesome CDN | FA is 100–400KB; individual inline SVGs are 0.1–0.5KB each; no request, no render flash |
## Final Size Budget
| Component | Inline Size | Notes |
|-----------|-------------|-------|
| highlight.js core | ~22.7KB | From jsdelivr `lib/core.min.js` |
| hljs languages (bash, yaml, json, markdown, shell) | ~9.2KB | 5 files combined |
| `github-dark` theme CSS | ~1.3KB | Verified on jsdelivr |
| Page CSS (custom theme, layout, tabs, collapsibles) | ~10–15KB | Estimate for full guide |
| Tab + copy JS | ~1KB | Hand-written, < 50 lines |
| HTML content | ~30–60KB | Depends on guide length |
| **Total** | **~75–110KB** | Single file, no requests after load |
## What NOT to Use
| Technology | Reason to Avoid |
|------------|----------------|
| React / Vue / Svelte | 40–300KB runtime for static content; component model adds zero value |
| jQuery | 87KB for `addClass`/`addEventListener` wrappers; vanilla JS 2024+ is cleaner |
| Bootstrap / Tailwind | CSS frameworks larger than the entire page content; no responsive grid needed for one-column doc |
| Webpack / Vite / Rollup / esbuild | Full bundlers are overkill; a 30-line Node script handles the inline-assembly need |
| CDN links in final HTML | Breaks the "self-contained" requirement; CDN unavailability = broken page |
| Google Fonts / external web fonts | Network request at open time; flash of unstyled text; privacy exposure |
| Font Awesome / icon CDN | 100–400KB for what 10 inline `<svg>` elements accomplish for free |
| highlight.js full bundle | 125KB includes 180+ unused languages; use core + 5 languages = 33KB |
| CSS-in-JS | Requires a JS runtime to generate styles; wrong tool for static HTML |
| localStorage for state | Overkill; tab/collapsible state doesn't need persistence for a reference guide |
## Sources
| Source | Confidence | Notes |
|--------|------------|-------|
| `https://api.github.com/repos/highlightjs/highlight.js/releases/latest` | HIGH | Confirmed v11.11.1, released 2024-12-25 |
| `https://cdn.jsdelivr.net/npm/highlight.js@11.11.1/` — individual file sizes | HIGH | Verified via HTTP HEAD/GET; all paths confirmed 200 OK |
| `https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.11.1/styles/github-dark.min.css` | HIGH | Verified content; GitHub Primer dark color palette confirmed |
| `https://api.github.com/repos/PrismJS/prism/releases/latest` | HIGH | Confirmed v1.30.0, released 2025-03-10 |
| MDN — `<details>/<summary>` element | HIGH | Browser-native, widely supported since 2020 |
| MDN — `Navigator.clipboard.writeText()` | HIGH | Supported in all modern browsers (Chrome 66+, Firefox 63+, Safari 13.1+) |
| CSS Custom Properties — MDN | HIGH | Baseline supported since 2017 |
<!-- GSD:stack-end -->

<!-- GSD:conventions-start source:CONVENTIONS.md -->
## Conventions

Conventions not yet established. Will populate as patterns emerge during development.
<!-- GSD:conventions-end -->

<!-- GSD:architecture-start source:ARCHITECTURE.md -->
## Architecture

Architecture not yet mapped. Follow existing patterns found in the codebase.
<!-- GSD:architecture-end -->

<!-- GSD:workflow-start source:GSD defaults -->
## GSD Workflow Enforcement

Before using Edit, Write, or other file-changing tools, start work through a GSD command so planning artifacts and execution context stay in sync.

Use these entry points:
- `/gsd-quick` for small fixes, doc updates, and ad-hoc tasks
- `/gsd-debug` for investigation and bug fixing
- `/gsd-execute-phase` for planned phase work

Do not make direct repo edits outside a GSD workflow unless the user explicitly asks to bypass it.
<!-- GSD:workflow-end -->



<!-- GSD:profile-start -->
## Developer Profile

> Profile not yet configured. Run `/gsd-profile-user` to generate your developer profile.
> This section is managed by `generate-claude-profile` -- do not edit manually.
<!-- GSD:profile-end -->

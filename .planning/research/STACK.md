# Technology Stack

**Project:** GitHub Copilot CLI — AI Agents & Skills Guide
**Researched:** 2025-04-10
**Constraint:** Single self-contained `.html` file — zero external runtime dependencies

---

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

**Total inlined syntax-highlighting budget:** ~33KB (core + 5 languages + theme)

**Source:** `https://cdn.jsdelivr.net/npm/highlight.js@11.11.1/` — all files verified present

**Why highlight.js over Prism.js:**
- The `github-dark` theme precisely matches GitHub's dark UI (background `#0d1117`, text `#c9d1d9`) — ideal for a GitHub Copilot guide
- Core + languages path on jsdelivr is clean: `lib/core.min.js`, `lib/languages/bash.min.js`, etc.
- Prism v1.30.0 (March 2025) is equally valid but lacks a native GitHub-branded theme; budget difference is negligible (~30KB vs 33KB)

---

### Interactivity

| Approach | Library/API | Why |
|----------|-------------|-----|
| Tabs | Vanilla JS (~30 lines) | No library needed; `data-target` attributes + CSS class toggling covers the full requirement |
| Collapsibles | Native `<details>/<summary>` + CSS | Zero JS; browser-native; animatable with `max-height` transition; `::marker` is customizable |
| Copy to clipboard | `Navigator.clipboard.writeText()` | Browser-native since 2018; no library; one function on each `<pre>` block |

**Why no JS framework (React/Vue/Svelte):**  
Frameworks add 40–300KB of runtime overhead for hydration, virtual DOM, and module resolution — none of which apply to static documentation content. Vanilla JS handles the two required interactions (tabs, copy) in under 50 lines.

---

### Styling

| Approach | Why |
|----------|-----|
| CSS Custom Properties (variables) | One-place theming — change `--bg-page` and everything updates; no preprocessor needed |
| No CSS framework | Bootstrap minified CSS is ~100KB; Tailwind with JIT is still 30–80KB purged. Both outweigh the entire rest of the page. Pure custom CSS is 5–15KB and purpose-built. |
| System font stack (UI) | No font download; renders crisply on every OS; immediately available |
| Monospace font stack (code) | Cascading to OS-native mono fonts avoids a 30–100KB web font download |

---

### Self-Containment Strategy

The file is assembled inline — no CDN links in production, no external requests in the browser:

```
Source HTML (template)
  └── <style> block  → inline all CSS (page styles + github-dark theme = ~15KB)
  └── <script> block → inline all JS (hljs core + 5 langs + tab/copy logic = ~35KB)
  └── Content        → narrative HTML markup
```

**Two valid assembly paths:**

**Path A — Direct authoring** (simplest for a one-time deliverable):  
Write the HTML file with `<script>` and `<style>` blocks already containing fetched/inlined content. Fetch CDN assets once, paste inline. No tooling required.

**Path B — Build script** (better for iteration):  
Maintain a `src/index.src.html` template with `<!-- INJECT:hljs-core -->` markers. A `build.js` Node script fetches CDN URLs, Base64-encodes nothing (they're text), and writes them inline. Run once to regenerate `dist/index.html`. Keeps source readable. Recommended if the guide will evolve past one draft.

---

## Color Palette: GitHub Dark Terminal Theme

These exact values power both the page chrome AND the `github-dark` highlight.js theme — they're drawn from GitHub's own production dark-mode CSS.

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

**Rationale:** This palette is GitHub's Primer "dark" color scale, verified against `github-dark.min.css` content. Code tokens, page chrome, and interactive elements share one coherent vocabulary — no jarring color-system mismatch between the page and code blocks.

---

## Typography

```css
/* UI / body copy */
font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
             Roboto, Helvetica, Arial, sans-serif;

/* Code, terminals, file paths */
font-family: 'SF Mono', 'Fira Code', 'Cascadia Code', 'Cascadia Mono',
             Consolas, 'Liberation Mono', monospace;
```

**Why system fonts only:** No web font requests = no FOUT, no network dependency, no privacy-sensitive CDN call. System monospace fonts (`SF Mono` on macOS, `Cascadia Code` on Windows, `Fira Code` via user config) are exactly what the target audience already has installed for their terminal.

---

## HTML Patterns

### Tabs

```html
<!-- Navigation -->
<nav class="tabs">
  <button class="tab-btn active" data-target="cli">CLI Intro</button>
  <button class="tab-btn" data-target="agents">Agents</button>
  <button class="tab-btn" data-target="skills">Skills</button>
</nav>

<!-- Panels -->
<div class="tab-panel active" id="tab-cli"> ... </div>
<div class="tab-panel hidden"  id="tab-agents"> ... </div>
<div class="tab-panel hidden"  id="tab-skills"> ... </div>
```

```js
// ~20 lines, no library
document.querySelectorAll('.tab-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
    document.querySelectorAll('.tab-panel').forEach(p => p.classList.add('hidden'));
    btn.classList.add('active');
    document.getElementById('tab-' + btn.dataset.target).classList.remove('hidden');
  });
});
```

### Collapsibles

```html
<details class="collapsible">
  <summary>
    <span class="summary-icon">▶</span>
    Creating an Agent Manually
  </summary>
  <div class="collapsible-body">
    <!-- content -->
  </div>
</details>
```

```css
/* Smooth animation — no JS needed */
details.collapsible summary { cursor: pointer; list-style: none; }
details.collapsible summary::marker { display: none; }
details.collapsible[open] .summary-icon { transform: rotate(90deg); }
.summary-icon { display: inline-block; transition: transform 0.2s ease; }
```

### Code Blocks with Copy Button

```html
<div class="code-wrapper">
  <button class="copy-btn" onclick="copyCode(this)">Copy</button>
  <pre><code class="language-bash">gh copilot suggest "list running containers"</code></pre>
</div>
```

```js
function copyCode(btn) {
  const code = btn.nextElementSibling.querySelector('code').innerText;
  navigator.clipboard.writeText(code).then(() => {
    btn.textContent = 'Copied!';
    setTimeout(() => btn.textContent = 'Copy', 2000);
  });
}
```

### highlight.js Initialization

```html
<!-- Inlined in <script> block — hljs core + languages + initiation -->
<script>
  // ... hljs core (22.7KB inline) ...
  // ... hljs bash (3.4KB inline) ...
  // ... hljs yaml (2.1KB inline) ...
  // etc.

  // Register languages and init
  hljs.registerLanguage('bash', hljsBash);
  hljs.registerLanguage('yaml', hljsYaml);
  hljs.registerLanguage('json', hljsJson);
  hljs.registerLanguage('markdown', hljsMarkdown);
  hljs.registerLanguage('shell', hljsShell);
  hljs.highlightAll();
</script>
```

---

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

---

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

This is lighter than most single-page framework apps (which commonly ship 300KB–1MB+ of JS). The file will open instantly from a filesystem double-click and share cleanly as an email attachment or Slack upload.

---

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

---

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

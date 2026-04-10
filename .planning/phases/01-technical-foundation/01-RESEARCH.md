# Phase 1: Technical Foundation - Research

**Researched:** 2026-04-10
**Domain:** Vanilla HTML/CSS/JS single-file document with syntax highlighting, tab navigation, scrollspy, copy-to-clipboard, collapsibles, and callout components
**Confidence:** HIGH - all technology choices are established, well-documented browser APIs; no experimental tech

---

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions

- **D-01:** Direct authoring - write `index.html` directly, inline all assets (highlight.js, CSS, JS) by hand. No build script. One file to edit forever.
- **D-02:** Minimal header bar - slim top bar with title and subtitle. Tabs sit just below it. No hero section.
- **D-03:** Page title text: `GitHub Copilot CLI - AI Agents & Skills Guide`
- **D-04:** Pre-authored HTML sidebar links - written by hand, not auto-generated from heading tags. Total control, no JS generation needed.
- **D-05:** Scrollspy only - sidebar highlights the active section as the user scrolls. Uses Intersection Observer API. No sticky/collapsible complexity for Phase 1.
- **D-06:** Polished - subtle gradients on callouts, smooth tab transitions (CSS), refined button hover states.
- **D-07:** Callout box style: left border accent. Colored left border per type: info=blue, warning=yellow, danger=red, tip=green

### the Agent's Discretion

- Tab transition animation style (fade, slide, or instant) - agent picks what looks best
- Exact color values within the GitHub Primer dark palette
- Copy button icon (SVG clipboard vs "Copy" text vs both)
- Sidebar width and typography sizing

### Deferred Ideas (OUT OF SCOPE)

None - discussion stayed within phase scope.
</user_constraints>

---

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| FOUND-01 | Single self-contained `.html` file with no external dependencies | Direct authoring (D-01); all assets fetched once from CDN during authoring and pasted inline into `<style>` + `<script>` blocks |
| FOUND-02 | All CSS, JavaScript, and syntax highlighting assets are inlined | highlight.js core + 5 lang packs + github-dark theme ~33KB inline in `<script>`/`<style>`; page CSS ~10-15KB; interaction JS ~1KB |
| FOUND-03 | Dark terminal-inspired theme based on GitHub Primer dark palette | Full CSS custom property palette documented in ARCHITECTURE.md; `--color-bg-canvas: #0d1117` base; `--color-text-primary: #e6edf3` |
| FOUND-04 | Three main tabs: CLI Intro, Agents, Skills | ARIA tablist pattern; three `<article role="tabpanel">` + three sidebar `<nav>` elements; JS ~20 lines with event delegation |
| FOUND-05 | Collapsible subsections via `<details>`/`<summary>` | Native HTML - zero JS required to open/close; CSS chevron rotation via `details[open]` selector |
| FOUND-06 | highlight.js 11.11.1 with github-dark theme | `hljs.highlightAll()` after registering 5 language packs; github-dark theme CSS inlined into `<style>` block |
| FOUND-07 | Every code block has a copy-to-clipboard button | `navigator.clipboard.writeText()` + 2s visual success state; event delegation; textarea fallback for edge cases |
| FOUND-08 | File size under 200KB uncompressed | Budget: ~33KB hljs + ~15KB CSS + ~1KB JS + ~15KB placeholder HTML = ~64KB - well under 200KB |
</phase_requirements>

---

## Summary

Phase 1 builds the complete HTML shell for the GitHub Copilot CLI reference guide. All technology choices are locked and well-understood: highlight.js 11.11.1 with github-dark theme (inlined), vanilla JS for tabs/scrollspy/copy (~50 lines total), native `<details>` for collapsibles, and bespoke CSS with Custom Properties for the GitHub Primer dark palette. No frameworks, no build tools, no CDN links in the final file.

The implementation is a single `index.html` with a `<style>` block (page CSS + github-dark theme), three `<article role="tabpanel">` elements, three pre-authored sidebar `<nav>` elements, and a `<script>` block (inlined hljs + 5 language packs + ~50 lines of vanilla JS). The key discipline is assembling this correctly - fetching CDN assets once during authoring, pasting them inline, and never leaving CDN URLs in the production file.

The UI-SPEC.md and ARCHITECTURE.md together provide a complete, authoritative blueprint. Every visual property, interaction contract, spacing value, color token, and component structure is fully specified. The planner should treat these as ground truth.

**Primary recommendation:** Follow the ARCHITECTURE.md build order exactly (layout -> typography -> tabs -> code blocks -> collapsibles -> callouts) so each layer can be verified before the next is added.

---

## Standard Stack

### Core

| Library | Version | Purpose | Why Standard |
|---------|---------|---------|--------------|
| highlight.js core | 11.11.1 | Syntax highlighting engine | ~22.7KB; core-only avoids 125KB full bundle; confirmed latest release (2024-12-25) |
| hljs `bash` language | 11.11.1 | CLI command highlighting | Primary language in this guide; ~3.4KB |
| hljs `yaml` language | 11.11.1 | Agent/skill frontmatter | YAML is the agent/skill file format; ~2.1KB |
| hljs `json` language | 11.11.1 | Config snippet highlighting | Lightweight ~0.7KB |
| hljs `markdown` language | 11.11.1 | .md file example display | Shows full agent/skill file examples; ~2.4KB |
| hljs `shell` language | 11.11.1 | Shell session highlighting | Distinguishes interactive sessions from scripts; ~0.6KB |
| hljs `github-dark` theme | 11.11.1 | Code block colors | GitHub Primer dark palette - exactly on-brand; ~1.3KB |

### Browser APIs (no install)

| API | Purpose | When to Use |
|-----|---------|-------------|
| `navigator.clipboard.writeText()` | Copy-to-clipboard | All copy button implementations; Chrome 66+, Firefox 63+, Safari 13.1+ |
| `IntersectionObserver` | Scrollspy | Sidebar active link highlighting; threshold: 0.4, rootMargin: "-56px 0px 0px 0px" |
| `<details>`/`<summary>` | Collapsibles | All expandable sections - zero JS required to function |
| CSS Custom Properties | Theming | All color/spacing/typography tokens in single `:root` block |
| CSS Grid | Layout | 260px sidebar + 1fr main; 56px header + 1fr body |

### Asset Fetch URLs (authoring only - NOT for production HTML)

```
highlight.js core:    https://cdn.jsdelivr.net/npm/highlight.js@11.11.1/lib/core.min.js
hljs bash:            https://cdn.jsdelivr.net/npm/highlight.js@11.11.1/lib/languages/bash.min.js
hljs yaml:            https://cdn.jsdelivr.net/npm/highlight.js@11.11.1/lib/languages/yaml.min.js
hljs json:            https://cdn.jsdelivr.net/npm/highlight.js@11.11.1/lib/languages/json.min.js
hljs markdown:        https://cdn.jsdelivr.net/npm/highlight.js@11.11.1/lib/languages/markdown.min.js
hljs shell:           https://cdn.jsdelivr.net/npm/highlight.js@11.11.1/lib/languages/shell.min.js
github-dark theme:    https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.11.1/styles/github-dark.min.css
```

**CRITICAL:** These URLs are fetch sources during authoring only. The final `index.html` must contain their content pasted inline. Zero `<link>` or `<script src="">` tags pointing to CDN in the production file.

### Size Budget

| Component | Inline Size | Notes |
|-----------|-------------|-------|
| highlight.js core | ~22.7KB | `lib/core.min.js` - verified |
| hljs languages (5) | ~9.2KB | bash + yaml + json + markdown + shell combined |
| github-dark theme CSS | ~1.3KB | Verified on cdnjs |
| Page CSS (custom) | ~10-15KB | Full component set + custom properties |
| Interactivity JS | ~1KB | < 50 lines hand-written |
| HTML content (placeholders) | ~5-15KB | Phase 1 uses placeholder text only |
| **Total** | **~50-65KB** | Well under 200KB with large margin |

---

## Architecture Patterns

### Recommended File Structure

```
index.html
  <head>
    <meta charset>, <meta viewport>, <title>
    <style>
      Section 1:  CSS Custom Properties (:root - all tokens)
      Section 2:  Reset / Base
      Section 3:  Typography
      Section 4:  Layout Grid (body CSS Grid shell)
      Section 5:  Site Header
      Section 6:  Tab Navigation
      Section 7:  Sidebar Navigation
      Section 8:  Content Panels (tab panel show/hide + opacity transition)
      Section 9:  Section Headings
      Section 10: Code Block Component
      Section 11: Collapsible Component
      Section 12: Callout Boxes (4 variants)
      Section 13: Inline Code
      Section 14: Tables
      Section 15: Scrollbar Styling
      [appended:  github-dark.min.css content]
    </style>
  </head>
  <body>
    <header class="site-header">          (brand + tablist)
    <div class="page-body">
      <aside class="sidebar">
        <nav id="sidebar-nav-cli-intro">  (pre-authored links)
        <nav id="sidebar-nav-agents" class="sidebar-nav--hidden">
        <nav id="sidebar-nav-skills" class="sidebar-nav--hidden">
      <main class="main-content">
        <article id="section-cli-intro" role="tabpanel" class="content-panel content-panel--active">
          <section id="[anchor]"> x N    (placeholder sections)
        <article id="section-agents" role="tabpanel" class="content-panel content-panel--hidden">
        <article id="section-skills" role="tabpanel" class="content-panel content-panel--hidden">
    <script>
      [inlined hljs core]
      [inlined hljs bash, yaml, json, markdown, shell]
      hljs.registerLanguage() x5 + hljs.highlightAll()
      function initTabs() { ... }
      function initCopyButtons() { ... }
      function initScrollspy() { ... }
      document.addEventListener('DOMContentLoaded', () => {
        initTabs(); initCopyButtons(); initScrollspy();
      });
  </body>
```

### Pattern 1: CSS Grid Layout Shell

**What:** Two-level grid - header spans full width, body row splits sidebar/main
**When to use:** The entire page layout

```css
/* Source: ARCHITECTURE.md Layout Grid section */
body {
  display: grid;
  grid-template-rows: var(--header-height) 1fr;
  grid-template-columns: var(--sidebar-width) 1fr;
  grid-template-areas:
    "header header"
    "sidebar main";
  height: 100vh;
  overflow: hidden;
}

.site-header { grid-area: header; }

.page-body {
  grid-column: 1 / -1;
  grid-row: 2;
  display: grid;
  grid-template-columns: var(--sidebar-width) 1fr;
  overflow: hidden;
}

.sidebar      { grid-area: sidebar; overflow-y: auto; }
.main-content { grid-area: main;    overflow-y: auto; }
```

### Pattern 2: ARIA Tab Navigation

**What:** Three-tab switcher using ARIA tablist pattern with 150ms opacity fade
**When to use:** Top-level tab switching between CLI Intro / Agents / Skills

```html
<!-- Source: ARCHITECTURE.md + UI-SPEC.md Tab Navigation section -->
<nav class="tab-nav" role="tablist" aria-label="Guide sections">
  <button class="tab-nav__tab tab-nav__tab--active"
          role="tab" aria-selected="true"
          aria-controls="section-cli-intro"
          id="tab-cli-intro"
          data-tab="cli-intro">CLI Intro</button>
  <button class="tab-nav__tab"
          role="tab" aria-selected="false"
          aria-controls="section-agents"
          id="tab-agents"
          data-tab="agents">Agents</button>
  <button class="tab-nav__tab"
          role="tab" aria-selected="false"
          aria-controls="section-skills"
          id="tab-skills"
          data-tab="skills">Skills</button>
</nav>
```

```css
/* Tab active state - accent underline, no border-left */
.tab-nav__tab--active {
  color: var(--color-text-primary);
  border-bottom: 2px solid var(--color-accent);
  font-weight: 600;
}

/* Opacity fade on panel show - 150ms per UI-SPEC */
.content-panel--active {
  display: block;
  opacity: 1;
  transition: opacity 0.15s ease-in;
}

.content-panel--hidden {
  display: none;
  opacity: 0;
}
```

```javascript
// Source: ARCHITECTURE.md Tab JS Pattern
function initTabs() {
  const tabNav = document.querySelector('.tab-nav');
  const panels = document.querySelectorAll('.content-panel');
  const sidebars = document.querySelectorAll('.sidebar-nav');

  tabNav.addEventListener('click', (e) => {
    const tab = e.target.closest('[data-tab]');
    if (!tab) return;
    const target = tab.dataset.tab;

    tabNav.querySelectorAll('[role="tab"]').forEach(t => {
      t.classList.toggle('tab-nav__tab--active', t.dataset.tab === target);
      t.setAttribute('aria-selected', t.dataset.tab === target);
    });

    panels.forEach(panel => {
      const isActive = panel.id === `section-${target}`;
      panel.classList.toggle('content-panel--active', isActive);
      panel.classList.toggle('content-panel--hidden', !isActive);
    });

    sidebars.forEach(nav => {
      nav.classList.toggle('sidebar-nav--hidden', nav.id !== `sidebar-nav-${target}`);
    });

    document.querySelector('.main-content').scrollTop = 0;
    document.querySelector('.sidebar').scrollTop = 0;
  });
}
```

### Pattern 3: Sidebar Active Link - Box Shadow Inset Trick

**What:** Active scrollspy indicator using box-shadow instead of border-left
**Why:** border-left would shift the text 3px; box-shadow consumes no layout space, padding stays at 16px

```css
/* Source: UI-SPEC.md Sidebar Navigation - Active scrollspy link */
.sidebar-nav__link--active {
  color: var(--color-text-link);
  box-shadow: inset 3px 0 0 var(--color-accent);
  font-weight: 600;
  /* padding-left: 16px unchanged - box-shadow does NOT affect layout */
}
```

### Pattern 4: Scrollspy with IntersectionObserver

**What:** Highlights sidebar link corresponding to the visible section
**When to use:** On scroll within any active tab panel

```javascript
// Source: ARCHITECTURE.md Scrollspy + UI-SPEC.md rootMargin specification
function initScrollspy() {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      const id = entry.target.id;
      const link = document.querySelector(`.sidebar-nav__link[href="#${id}"]`);
      if (!link) return;

      if (entry.isIntersecting) {
        // Deactivate all links in visible sidebar
        const activeSidebar = document.querySelector('.sidebar-nav:not(.sidebar-nav--hidden)');
        activeSidebar?.querySelectorAll('.sidebar-nav__link--active')
          .forEach(l => l.classList.remove('sidebar-nav__link--active'));
        link.classList.add('sidebar-nav__link--active');
      }
    });
  }, {
    threshold: 0.4,
    rootMargin: '-56px 0px 0px 0px'   // accounts for 56px sticky header
  });

  document.querySelectorAll('.content-section[id]').forEach(s => observer.observe(s));
}
```

### Pattern 5: Code Block Component

**What:** Wrapper div with header bar (lang label + copy button) + pre/code body
**When to use:** Every code example

```html
<!-- Source: ARCHITECTURE.md Code Block Component -->
<div class="code-block" data-lang="bash">
  <div class="code-block__header">
    <span class="code-block__lang">bash</span>
    <button class="code-block__copy" aria-label="Copy to clipboard">
      <svg class="icon icon--copy" aria-hidden="true"><!-- clipboard SVG 14x14 --></svg>
      <svg class="icon icon--check icon--hidden" aria-hidden="true"><!-- checkmark SVG 14x14 --></svg>
      <span class="code-block__copy-label">Copy code</span>
    </button>
  </div>
  <pre class="code-block__pre"><code class="language-bash">gh copilot suggest "list running containers"</code></pre>
</div>
```

```javascript
// Source: ARCHITECTURE.md Copy Button JS Pattern
function initCopyButtons() {
  document.addEventListener('click', async (e) => {
    const btn = e.target.closest('.code-block__copy');
    if (!btn) return;

    const code = btn.closest('.code-block').querySelector('code').textContent;

    try {
      await navigator.clipboard.writeText(code);
    } catch {
      // Textarea fallback for blocked clipboard API
      const ta = document.createElement('textarea');
      ta.value = code;
      ta.style.cssText = 'position:fixed;opacity:0';
      document.body.appendChild(ta);
      ta.select();
      document.execCommand('copy');
      document.body.removeChild(ta);
    }

    // Visual success state - 2000ms per UI-SPEC
    const label = btn.querySelector('.code-block__copy-label');
    const iconCopy = btn.querySelector('.icon--copy');
    const iconCheck = btn.querySelector('.icon--check');
    btn.classList.add('code-block__copy--copied');
    if (label) label.textContent = 'Copied!';
    if (iconCopy) iconCopy.classList.add('icon--hidden');
    if (iconCheck) iconCheck.classList.remove('icon--hidden');

    setTimeout(() => {
      btn.classList.remove('code-block__copy--copied');
      if (label) label.textContent = 'Copy code';
      if (iconCopy) iconCopy.classList.remove('icon--hidden');
      if (iconCheck) iconCheck.classList.add('icon--hidden');
    }, 2000);
  });
}
```

### Pattern 6: Callout Box (4 Variants)

**What:** Left-border accent box with colored type label, 8% opacity background tint
**When to use:** Info, warnings, danger notices, and tips

```html
<!-- Source: UI-SPEC.md Callout Box Component -->
<div class="callout callout--info">
  <span class="callout__type">Info</span>
  <p>Callout content goes here.</p>
</div>
<!-- Variants: callout--warning | callout--danger | callout--tip -->
```

```css
/* Source: UI-SPEC.md Color table + ARCHITECTURE.md Callout CSS */
.callout {
  border-left: 4px solid;
  border-radius: 0 6px 6px 0;
  padding: 12px 16px;
  margin: 16px 0;
}
.callout__type {
  display: block;
  font-size: 13px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  margin-bottom: 4px;
}

.callout--info    { border-color: #58a6ff; background: rgba(88, 166, 255, 0.08); }
.callout--info    .callout__type { color: #58a6ff; }
.callout--warning { border-color: #e3b341; background: rgba(227, 179, 65, 0.08); }
.callout--warning .callout__type { color: #e3b341; }
.callout--danger  { border-color: #f85149; background: rgba(248, 81, 73, 0.08); }
.callout--danger  .callout__type { color: #f85149; }
.callout--tip     { border-color: #3fb950; background: rgba(63, 185, 80, 0.08); }
.callout--tip     .callout__type { color: #3fb950; }
```

### Pattern 7: Collapsible (details/summary)

**What:** Native HTML5 collapsible with CSS chevron rotation; zero JS required
**When to use:** Expandable subsections, reference tables

```html
<!-- Source: ARCHITECTURE.md Collapsible Component -->
<details class="collapsible">
  <summary class="collapsible__trigger">
    <svg class="collapsible__chevron" aria-hidden="true"><!-- right-pointing chevron --></svg>
    Section Title
  </summary>
  <div class="collapsible__body">
    <!-- content -->
  </div>
</details>
```

```css
/* Chevron rotation on open - no JS needed */
.collapsible__trigger { list-style: none; cursor: pointer; }
.collapsible__trigger::-webkit-details-marker { display: none; } /* Safari */
.collapsible__chevron { transition: transform 0.2s ease; }
details[open] .collapsible__chevron { transform: rotate(90deg); }
```

### Pattern 8: highlight.js Initialization

**What:** Register language packs then call highlightAll - must run after DOM is ready

```javascript
// Source: ARCHITECTURE.md Syntax Highlighting section
// [hljs core is inlined above this point in the same <script> block]
// [language pack functions (hljsBash, hljsYaml, etc.) are inlined above]

hljs.registerLanguage('bash', hljsBash);
hljs.registerLanguage('yaml', hljsYaml);
hljs.registerLanguage('json', hljsJson);
hljs.registerLanguage('markdown', hljsMarkdown);
hljs.registerLanguage('shell', hljsShell);
hljs.highlightAll();
// hljs.highlightAll() runs on DOMContentLoaded automatically
// OR call explicitly: document.querySelectorAll('pre code').forEach(el => hljs.highlightElement(el));
```

### Recommended Build Order

Build in this sequence to validate each layer before adding complexity:

1. **Base Layout** - CSS custom properties + CSS Grid shell + header/sidebar/main structure
2. **Theme + Typography** - All font tokens, base element styles, scrollbar styling
3. **Tab Navigation** - Tab CSS + tab-switching JS + sidebar show/hide
4. **Scrollspy** - IntersectionObserver connecting sections to sidebar links
5. **Code Block Component** - HTML structure + CSS + copy button JS + inlined hljs
6. **Collapsible Component** - details/summary CSS (chevron rotation)
7. **Callout Boxes** - 4 variants CSS
8. **Placeholder Content** - One sample of each component per tab for verification

### Anti-Patterns to Avoid

- **CDN links in production HTML:** Any `<link href="https://...">` or `<script src="https://...">` in index.html breaks offline use. All assets must be pasted inline.
- **Inline styles for colors:** `color: #3fb950` directly on elements makes theme changes require global search-replace. Use CSS custom property tokens only.
- **Full highlight.js bundle:** `highlight.min.js` is 125KB. Use `core.min.js` + individual language files = 33KB.
- **Sections without ID attributes:** Every `<section>` needs an `id` in kebab-case matching the sidebar link's `href`. Missing IDs silently break scrollspy.
- **Monolithic script block:** All JS in one undifferentiated blob causes initialization order issues. Use named `init*()` functions called from a single `DOMContentLoaded` handler.
- **border-left for active sidebar indicator:** Shifts text 3px, breaking layout consistency. Use `box-shadow: inset 3px 0 0 var(--color-accent)` instead.
- **`innerText` for copy content:** Use `.textContent` (not `.innerText`) to extract code - `innerText` is layout-dependent and may fail in hidden panels.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Syntax highlighting | Custom regex token highlighter | highlight.js 11.11.1 | Edge cases in bash, YAML, markdown quoting are extensive; hljs handles them |
| Copy to clipboard | Flash/ZeroClipboard or custom selection | `navigator.clipboard.writeText()` | Browser-native since 2018; supported in all modern browsers |
| Collapsibles with animation | JS-driven show/hide with maxHeight calculations | Native `<details>`/`<summary>` + CSS chevron | Native HTML5; accessible by default; browser handles all state |
| Font rendering | Web font downloads | System font stack | No request, no FOUT, developer audience has good monospace fonts |
| Scroll detection | `scroll` event listeners with debounce | `IntersectionObserver` API | More performant; fires on visibility not scroll position; exact threshold control |

**Key insight:** Every "custom solution" in this domain has a better browser-native equivalent. The entire interactive layer (tabs + scrollspy + copy + collapsibles) should be < 50 lines of vanilla JS.

---

## Common Pitfalls

### Pitfall 1: CDN Links Left in Production HTML
**What goes wrong:** Developer writes `<script src="https://cdn.jsdelivr.net/...">` as a placeholder, forgets to inline, ships the file. Works on their machine (online), fails offline.
**Why it happens:** It's faster to test with CDN links, easy to forget the inline step.
**How to avoid:** Never write a CDN `<script src>` or `<link href>` in index.html at any point. Fetch CDN content immediately and paste inline as you build each component.
**Warning signs:** Any `src="http` or `href="http` in the final file.

### Pitfall 2: highlight.js Full Bundle Instead of Core
**What goes wrong:** Developer inlines `highlight.min.js` (125KB) instead of `core.min.js` + 5 language files (~33KB). File is 92KB larger than necessary.
**Why it happens:** The full bundle is easier (one file), and the CDN path difference is subtle.
**How to avoid:** Always use `lib/core.min.js` + `lib/languages/{lang}.min.js` paths on jsdelivr.
**Warning signs:** File size > 150KB before any content is added.

### Pitfall 3: Sections Without ID Attributes
**What goes wrong:** Scrollspy observer registers sections, but `querySelectorAll('.sidebar-nav__link[href="#missing-id"]')` returns null. No sidebar links activate. No error thrown.
**Why it happens:** Easy to forget the `id` attribute when authoring content quickly.
**How to avoid:** Every `<section>` in a content panel MUST have an `id` in kebab-case. Add sidebar `<a href="#...">` at the same time as the `<section id="...">`.
**Warning signs:** Sidebar links never highlight during scrolling.

### Pitfall 4: Tab Panel Opacity Transition with display:none
**What goes wrong:** CSS transition on `opacity` doesn't animate when switching from `display:none` to `display:block` - the property change is instantaneous.
**Why it happens:** `display` and `opacity` transitions don't compose naturally.
**How to avoid:** For the 150ms fade-in on tab switch: add `.content-panel--active` class (sets `display:block`), then trigger reflow, then add opacity class. Or: keep panels as `visibility:hidden; opacity:0` instead of `display:none` while inactive (acceptable since main area is scrollable anyway, and inactive panels don't take layout space in the grid).
**Warning signs:** Tab switch looks instant with no fade, even though transition CSS is present.

### Pitfall 5: Copy Button Using innerText on Hidden Panels
**What goes wrong:** `codeEl.innerText` returns empty string for elements inside `display:none` containers (hidden tab panels).
**Why it happens:** `innerText` is layout-aware - if the element isn't rendered, it returns "".
**How to avoid:** Use `.textContent` instead of `.innerText` for extracting code block content.
**Warning signs:** Copy button copies empty string on code blocks in non-active tabs.

### Pitfall 6: Scrollspy rootMargin Missing Header Offset
**What goes wrong:** Sections trigger as "visible" before they are actually visible in the viewport (behind the 56px sticky header). Wrong sidebar link activates.
**Why it happens:** IntersectionObserver measures against the viewport, not the visible content area.
**How to avoid:** Always set `rootMargin: '-56px 0px 0px 0px'` to account for the 56px header height. This matches the `--header-height` CSS token.
**Warning signs:** Active sidebar link is always one section behind where user actually is.

---

## Code Examples

### Complete CSS Custom Properties Block

```css
/* Source: ARCHITECTURE.md Dark Theme Color Palette */
:root {
  /* Backgrounds */
  --color-bg-canvas:     #0d1117;
  --color-bg-surface:    #161b22;
  --color-bg-overlay:    #1c2128;
  --color-bg-subtle:     #21262d;

  /* Borders */
  --color-border:        #30363d;
  --color-border-muted:  #21262d;
  --color-border-subtle: #484f58;

  /* Text */
  --color-text-primary:  #e6edf3;
  --color-text-secondary:#8b949e;
  --color-text-link:     #58a6ff;

  /* Accent */
  --color-accent:        #3fb950;
  --color-accent-subtle: #1a3a25;

  /* Semantic */
  --color-success:       #3fb950;
  --color-warning:       #e3b341;
  --color-danger:        #f85149;
  --color-info:          #58a6ff;

  /* Spacing (8-point sub-grid) */
  --spacing-xs:  4px;
  --spacing-sm:  8px;
  --spacing-md:  16px;
  --spacing-lg:  24px;
  --spacing-xl:  32px;
  --spacing-2xl: 48px;

  /* Typography */
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
  --font-mono: "SFMono-Regular", Consolas, "Liberation Mono", Menlo, Courier, monospace;
  --font-size-base: 15px;
  --font-size-sm:   13px;
  --font-size-lg:   17px;
  --font-size-xl:   20px;
  --line-height-base:  1.6;
  --line-height-tight: 1.3;
  --line-height-code:  1.5;

  /* Layout */
  --sidebar-width:     260px;
  --header-height:     56px;
  --content-max-width: 860px;
  --border-radius:     6px;
  --border-radius-lg:  12px;
}
```

### Type Scale Implementation

```css
/* Source: UI-SPEC.md Typography section */
body {
  font-family: var(--font-sans);
  font-size: var(--font-size-base);   /* 15px */
  font-weight: 400;
  line-height: var(--line-height-base);
  color: var(--color-text-primary);
  background: var(--color-bg-canvas);
}

h1 { font-size: 20px; font-weight: 600; line-height: 1.2; }
h2, h3 { font-size: 17px; font-weight: 600; line-height: 1.3; }
/* Labels, captions, sidebar links, copy button text, code lang badge */
.label { font-size: 13px; }
code, pre { font-family: var(--font-mono); font-size: 13px; line-height: 1.5; }
```

### Inline Code in Prose

```css
/* Source: UI-SPEC.md - Inline code uses overlay background + 4px horizontal padding */
:not(pre) > code {
  font-family: var(--font-mono);
  font-size: 13px;
  background: var(--color-bg-overlay);
  padding: 2px 4px;
  border-radius: 3px;
  color: var(--color-text-primary);
}
```

### Placeholder Section Template (per tab)

```html
<!-- Source: ARCHITECTURE.md Section Structure Template -->
<!-- Use this for all three tabs in Phase 1 -->
<section id="[tab]-overview" class="content-section">
  <h2>[Tab] Overview</h2>
  <p>Content for this section will be added in Phase 2.</p>

  <!-- Sample code block (verify highlight.js works) -->
  <div class="code-block" data-lang="bash">
    <div class="code-block__header">
      <span class="code-block__lang">bash</span>
      <button class="code-block__copy" aria-label="Copy to clipboard">
        <svg class="icon icon--copy" aria-hidden="true" width="14" height="14"><!-- clipboard --></svg>
        <svg class="icon icon--check icon--hidden" aria-hidden="true" width="14" height="14"><!-- check --></svg>
        <span class="code-block__copy-label">Copy code</span>
      </button>
    </div>
    <pre class="code-block__pre"><code class="language-bash">copilot -p "placeholder example command"</code></pre>
  </div>

  <!-- Sample collapsible (verify open/close works) -->
  <details class="collapsible">
    <summary class="collapsible__trigger">
      <svg class="collapsible__chevron" aria-hidden="true"><!-- chevron --></svg>
      Reference Details
    </summary>
    <div class="collapsible__body">
      <p>Content for this section will be added in Phase 2.</p>
    </div>
  </details>

  <!-- Sample callout (verify all 4 variants) -->
  <div class="callout callout--info">
    <span class="callout__type">Info</span>
    <p>Sample callout - content will be added in Phase 2.</p>
  </div>
</section>
```

---

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|-----------------|--------------|--------|
| `scroll` event + `getBoundingClientRect()` for scrollspy | `IntersectionObserver` API | Chrome 58+ (2017), widespread 2019+ | No performance concerns; more precise; threshold-based |
| Flash / ZeroClipboard for copy | `navigator.clipboard.writeText()` | Chrome 66+, Firefox 63+ (2018) | No Flash dependency; fully async; Promise-based |
| CSS preprocessors (Sass/Less) for variables | CSS Custom Properties | Baseline 2017 | No build step; runtime-changeable; just as powerful for single theme |
| `<div>` + JS for accordions | Native `<details>`/`<summary>` | HTML5, widespread 2020+ | Zero JS; accessible by default; browser handles state |

---

## Open Questions

1. **Opacity transition on tab switch**
   - What we know: `display:none` + `opacity` transition don't compose directly (UI-SPEC says 150ms ease-in)
   - What's unclear: Best approach - CSS class juggling with requestAnimationFrame? visibility:hidden? CSS animation?
   - Recommendation: Use `visibility: hidden; pointer-events: none; position: absolute;` for inactive panels to allow opacity transition, OR use a single-frame `setTimeout` after adding `display:block` before adding the opacity class. Either approach is < 5 lines.

2. **highlight.js `highlightAll()` timing**
   - What we know: hljs.highlightAll() should fire after DOM is ready; it auto-attaches to DOMContentLoaded
   - What's unclear: Whether calling it inside the inline `<script>` at bottom of `<body>` is sufficient, or needs explicit DOMContentLoaded wrapper
   - Recommendation: Put hljs registration + `hljs.highlightAll()` inside the same `DOMContentLoaded` handler with the other init functions. Safest and most explicit.

---

## Environment Availability

> Step 2.6: SKIPPED (no external dependencies identified - this phase creates a static HTML file using only browser APIs and inlined library content; no CLI tools, databases, or services required)

---

## Sources

### Primary (HIGH confidence)

- STACK.md (`.planning/research/STACK.md`) - Technology choices, size budgets, CDN paths verified against live jsdelivr/cdnjs endpoints
- ARCHITECTURE.md (`.planning/research/ARCHITECTURE.md`) - HTML structure, CSS architecture, all component JS/CSS patterns, build order
- UI-SPEC.md (`.planning/phases/01-technical-foundation/01-UI-SPEC.md`) - Exact color values, spacing tokens, interaction contracts, copy/paste-ready component specs
- CONTEXT.md (`.planning/phases/01-technical-foundation/01-CONTEXT.md`) - Locked decisions D-01 through D-07
- MDN - IntersectionObserver API: https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API
- MDN - Clipboard API: https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API
- MDN - details/summary: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details
- W3C ARIA tab pattern: https://www.w3.org/WAI/ARIA/apg/patterns/tabs/

### Secondary (MEDIUM confidence)

- PITFALLS.md (`.planning/research/PITFALLS.md`) - Domain pitfalls for self-contained HTML guides; CDN risks, file size warnings, content accuracy warnings

---

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH - verified against live CDN endpoints; exact versions confirmed in STACK.md sources table
- Architecture: HIGH - documented patterns from ARCHITECTURE.md; all browser APIs well-established
- Pitfalls: HIGH - PITFALLS.md is the authoritative pitfall reference for this project

**Research date:** 2026-04-10
**Valid until:** 2026-07-10 (stable technology; highlight.js 11.x is stable; browser APIs are baseline)

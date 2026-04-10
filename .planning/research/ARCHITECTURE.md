# Architecture Patterns

**Domain:** Single-page interactive HTML developer guide
**Researched:** 2025-01-10
**Confidence:** HIGH — established HTML/CSS/JS patterns, no experimental tech

---

## Recommended Architecture

### Big Picture

```
┌─────────────────────────────────────────────────────────────┐
│  <header>  branding + section tabs                           │
├──────────────┬──────────────────────────────────────────────┤
│              │  <main>                                       │
│  <nav>       │  ┌──────────────────────────────────────┐   │
│  (sidebar)   │  │  <article id="cli-intro">             │   │
│              │  │    sections + collapsibles            │   │
│  • CLI Intro │  ├──────────────────────────────────────┤   │
│    ▸ Cmds    │  │  <article id="agents">                │   │
│    ▸ Ext     │  │    sections + collapsibles            │   │
│  • Agents    │  ├──────────────────────────────────────┤   │
│    ▸ Create  │  │  <article id="skills">                │   │
│    ▸ Orch    │  │    sections + collapsibles            │   │
│  • Skills    │  └──────────────────────────────────────┘   │
│    ▸ Files   │                                               │
│    ▸ Invoke  │                                               │
└──────────────┴──────────────────────────────────────────────┘
```

**Navigation model:** Tab-switch at top + sidebar deep-link anchors.

- The 3 top tabs (`CLI Intro` / `Agents` / `Skills`) show/hide the corresponding `<article>`. Only one article is visible at a time.
- The sidebar links deep-link to `<section>` anchors *within* the active article.
- Scrollspy highlights the active sidebar link as user scrolls through the active article.

**Why this over full-scroll single-page:**
The 3 segments are conceptually distinct acts (learn CLI → build agents → build skills). Tabs prevent accidental overwhelm from a massive content wall, and match the project requirement: "tabs between topics." Experienced developers tab to the section they need rather than reading top-to-bottom.

---

## HTML Semantic Structure

```html
<!DOCTYPE html>
<html lang="en" data-theme="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GitHub Copilot CLI — AI Agents & Skills Guide</title>
  <style>/* ALL CSS INLINE — see CSS Architecture */</style>
</head>
<body>

  <!-- ① TOP HEADER -->
  <header class="site-header">
    <div class="site-header__brand">
      <svg><!-- Copilot/Octocat icon SVG inline --></svg>
      <span class="site-header__title">GitHub Copilot CLI</span>
      <span class="site-header__subtitle">Agents &amp; Skills Guide</span>
    </div>
    <!-- Tab switcher — controls which <article> is visible -->
    <nav class="tab-nav" role="tablist" aria-label="Guide sections">
      <button class="tab-nav__tab tab-nav__tab--active"
              role="tab" aria-selected="true"
              aria-controls="section-cli-intro"
              data-tab="cli-intro">CLI Intro</button>
      <button class="tab-nav__tab"
              role="tab" aria-selected="false"
              aria-controls="section-agents"
              data-tab="agents">Agents</button>
      <button class="tab-nav__tab"
              role="tab" aria-selected="false"
              aria-controls="section-skills"
              data-tab="skills">Skills</button>
    </nav>
  </header>

  <!-- ② PAGE BODY: sidebar + main -->
  <div class="page-body">

    <!-- ③ SIDEBAR NAV — deep-links within active article -->
    <aside class="sidebar" aria-label="Section navigation">
      <!-- Populated dynamically by JS from active article's <section> headings -->
      <!-- OR authored as three nav lists, shown/hidden with tabs -->
      <nav class="sidebar-nav" id="sidebar-nav-cli-intro" aria-label="CLI Intro navigation">
        <ul class="sidebar-nav__list">
          <li class="sidebar-nav__item">
            <a class="sidebar-nav__link sidebar-nav__link--active"
               href="#cli-overview">Overview</a>
          </li>
          <li class="sidebar-nav__item sidebar-nav__item--group">
            <span class="sidebar-nav__group-label">Core Commands</span>
            <ul class="sidebar-nav__sublist">
              <li><a class="sidebar-nav__link" href="#cli-suggest">gh copilot suggest</a></li>
              <li><a class="sidebar-nav__link" href="#cli-explain">gh copilot explain</a></li>
            </ul>
          </li>
        </ul>
      </nav>
      <nav class="sidebar-nav sidebar-nav--hidden" id="sidebar-nav-agents" aria-label="Agents navigation">
        <!-- agents nav items -->
      </nav>
      <nav class="sidebar-nav sidebar-nav--hidden" id="sidebar-nav-skills" aria-label="Skills navigation">
        <!-- skills nav items -->
      </nav>
    </aside>

    <!-- ④ MAIN CONTENT — three articles, one visible at a time -->
    <main class="main-content" id="main-content">

      <article id="section-cli-intro"
               role="tabpanel"
               aria-labelledby="tab-cli-intro"
               class="content-panel content-panel--active">

        <section id="cli-overview" class="content-section">
          <h2 class="section-heading">Overview</h2>
          <!-- narrative text -->

          <!-- Code block component (see below) -->
          <div class="code-block" data-lang="bash">
            <div class="code-block__header">
              <span class="code-block__lang">bash</span>
              <!-- optional: <span class="code-block__filepath">.github/agents/my-agent.md</span> -->
              <button class="code-block__copy" aria-label="Copy code">
                <svg><!-- clipboard icon --></svg>
                <span>Copy</span>
              </button>
            </div>
            <pre><code class="language-bash">gh copilot suggest "list all running docker containers"</code></pre>
          </div>

          <!-- Collapsible subsection (see below) -->
          <details class="collapsible" id="cli-flags-reference">
            <summary class="collapsible__trigger">
              <svg class="collapsible__icon"><!-- chevron --></svg>
              Flag Reference
            </summary>
            <div class="collapsible__body">
              <!-- reference content here -->
            </div>
          </details>
        </section>

        <section id="cli-suggest" class="content-section">
          <!-- ... -->
        </section>

      </article>

      <article id="section-agents"
               role="tabpanel"
               aria-labelledby="tab-agents"
               class="content-panel content-panel--hidden">
        <!-- agents content -->
      </article>

      <article id="section-skills"
               role="tabpanel"
               aria-labelledby="tab-skills"
               class="content-panel content-panel--hidden">
        <!-- skills content -->
      </article>

    </main>
  </div>

  <script>/* ALL JS INLINE — see JavaScript Components */</script>
</body>
</html>
```

---

## CSS Architecture

### Single `<style>` block structure (author order matters)

```css
/* ─── 1. CUSTOM PROPERTIES (theme tokens) ───────────────────── */
/* ─── 2. RESET / BASE ───────────────────────────────────────── */
/* ─── 3. TYPOGRAPHY ─────────────────────────────────────────── */
/* ─── 4. LAYOUT (grid shell) ────────────────────────────────── */
/* ─── 5. HEADER ─────────────────────────────────────────────── */
/* ─── 6. TAB NAV ────────────────────────────────────────────── */
/* ─── 7. SIDEBAR ────────────────────────────────────────────── */
/* ─── 8. CONTENT PANEL ──────────────────────────────────────── */
/* ─── 9. SECTION HEADINGS ───────────────────────────────────── */
/* ─── 10. CODE BLOCK COMPONENT ──────────────────────────────── */
/* ─── 11. COLLAPSIBLE COMPONENT ─────────────────────────────── */
/* ─── 12. CALLOUT / NOTE BOXES ──────────────────────────────── */
/* ─── 13. INLINE CODE ───────────────────────────────────────── */
/* ─── 14. TABLES ────────────────────────────────────────────── */
/* ─── 15. SCROLLBAR STYLING ─────────────────────────────────── */
/* ─── 16. RESPONSIVE / MOBILE ───────────────────────────────── */
/* ─── 17. PRISM.JS THEME OVERRIDES ──────────────────────────── */
/* ─── 18. UTILITY CLASSES ───────────────────────────────────── */
/* ─── 19. ANIMATIONS ────────────────────────────────────────── */
```

### Dark Theme Color Palette

Based on GitHub's own dark mode palette — familiar to the target audience, rigorously tested for contrast.

```css
:root {
  /* ── Backgrounds ── */
  --color-bg-canvas:     #0d1117;   /* page background */
  --color-bg-surface:    #161b22;   /* sidebar, cards, header */
  --color-bg-overlay:    #1c2128;   /* code blocks, elevated surfaces */
  --color-bg-subtle:     #21262d;   /* hover states, striped rows */
  --color-bg-muted:      #30363d;   /* dividers, input backgrounds */

  /* ── Borders ── */
  --color-border:        #30363d;   /* default borders */
  --color-border-muted:  #21262d;   /* subtle separators */
  --color-border-subtle: #484f58;   /* active/focus borders */

  /* ── Text ── */
  --color-text-primary:  #e6edf3;   /* headings, body */
  --color-text-secondary:#8b949e;   /* muted labels, captions */
  --color-text-tertiary: #6e7681;   /* placeholders, deemphasized */
  --color-text-link:     #58a6ff;   /* hyperlinks */
  --color-text-code:     #e6edf3;   /* code tokens */

  /* ── Accent — terminal green (primary CTA, active states) ── */
  --color-accent:        #3fb950;   /* active tab underline, copy success */
  --color-accent-subtle: #1a3a25;   /* accent backgrounds */

  /* ── Semantic ── */
  --color-success:       #3fb950;
  --color-warning:       #e3b341;
  --color-danger:        #f85149;
  --color-info:          #58a6ff;

  /* ── Syntax tokens (Prism overrides) ── */
  --syn-keyword:         #ff7b72;   /* red — keywords, control flow */
  --syn-string:          #a5d6ff;   /* blue — strings */
  --syn-comment:         #8b949e;   /* gray — comments */
  --syn-function:        #d2a8ff;   /* purple — functions/commands */
  --syn-number:          #79c0ff;   /* light blue — numbers, flags */
  --syn-operator:        #ff7b72;   /* red — operators */
  --syn-property:        #79c0ff;   /* YAML keys */
  --syn-punctuation:     #8b949e;   /* brackets, colons */

  /* ── Spacing ── */
  --spacing-xs:  4px;
  --spacing-sm:  8px;
  --spacing-md:  16px;
  --spacing-lg:  24px;
  --spacing-xl:  32px;
  --spacing-2xl: 48px;

  /* ── Typography ── */
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
  --font-mono: "SFMono-Regular", Consolas, "Liberation Mono", Menlo, Courier, monospace;
  --font-size-base: 15px;
  --font-size-sm:   13px;
  --font-size-lg:   17px;
  --line-height-base: 1.6;
  --line-height-tight: 1.3;

  /* ── Layout ── */
  --sidebar-width:    260px;
  --header-height:    56px;
  --content-max-width: 860px;
  --border-radius:    6px;
  --border-radius-lg: 12px;
}
```

### Layout Grid

```css
/* Page shell — CSS Grid */
body {
  display: grid;
  grid-template-rows: var(--header-height) 1fr;
  grid-template-columns: var(--sidebar-width) 1fr;
  grid-template-areas:
    "header header"
    "sidebar main";
  height: 100vh;
  overflow: hidden;               /* prevent body scroll */
}

.site-header { grid-area: header; }

.page-body {
  grid-column: 1 / -1;           /* spans sidebar + main */
  grid-row: 2;
  display: grid;
  grid-template-columns: var(--sidebar-width) 1fr;
  grid-template-areas: "sidebar main";
  overflow: hidden;
}

.sidebar       { grid-area: sidebar; overflow-y: auto; }
.main-content  { grid-area: main;    overflow-y: auto; }
```

### Responsive Breakpoints

```css
/* Tablet: hide sidebar, move tab nav to top, hamburger toggle */
@media (max-width: 768px) {
  body {
    grid-template-columns: 1fr;
    grid-template-areas:
      "header"
      "main";
  }
  .sidebar {
    position: fixed;
    left: -100%;
    top: var(--header-height);
    width: var(--sidebar-width);
    transition: left 0.2s ease;
    z-index: 100;
  }
  .sidebar.sidebar--open { left: 0; }
}
```

---

## Component Patterns

### 1. Tab Navigation

**HTML Pattern:**
```html
<nav class="tab-nav" role="tablist">
  <button class="tab-nav__tab tab-nav__tab--active"
          role="tab" aria-selected="true"
          data-tab="cli-intro">CLI Intro</button>
  <button class="tab-nav__tab"
          role="tab" aria-selected="false"
          data-tab="agents">Agents</button>
  <button class="tab-nav__tab"
          role="tab" aria-selected="false"
          data-tab="skills">Skills</button>
</nav>
```

**CSS Pattern:**
```css
.tab-nav {
  display: flex;
  align-items: center;
  gap: 0;
  border-bottom: 1px solid var(--color-border);
}

.tab-nav__tab {
  padding: var(--spacing-sm) var(--spacing-lg);
  background: none;
  border: none;
  border-bottom: 2px solid transparent;
  color: var(--color-text-secondary);
  font-size: var(--font-size-base);
  cursor: pointer;
  transition: color 0.15s, border-color 0.15s;
}

.tab-nav__tab:hover {
  color: var(--color-text-primary);
  background: var(--color-bg-subtle);
}

.tab-nav__tab--active {
  color: var(--color-text-primary);
  border-bottom-color: var(--color-accent);
  font-weight: 600;
}
```

**JS Pattern:**
```javascript
function initTabs() {
  const tabNav = document.querySelector('.tab-nav');
  const panels = document.querySelectorAll('.content-panel');
  const sidebars = document.querySelectorAll('.sidebar-nav');

  tabNav.addEventListener('click', (e) => {
    const tab = e.target.closest('[data-tab]');
    if (!tab) return;

    const target = tab.dataset.tab;

    // Update tab active states
    tabNav.querySelectorAll('[role="tab"]').forEach(t => {
      t.classList.toggle('tab-nav__tab--active', t.dataset.tab === target);
      t.setAttribute('aria-selected', t.dataset.tab === target);
    });

    // Show/hide panels
    panels.forEach(panel => {
      panel.classList.toggle('content-panel--active',
        panel.id === `section-${target}`);
      panel.classList.toggle('content-panel--hidden',
        panel.id !== `section-${target}`);
    });

    // Show/hide sidebars
    sidebars.forEach(nav => {
      nav.classList.toggle('sidebar-nav--hidden',
        nav.id !== `sidebar-nav-${target}`);
    });

    // Reset scroll position
    document.querySelector('.main-content').scrollTop = 0;
    document.querySelector('.sidebar').scrollTop = 0;
  });
}
```

---

### 2. Code Block Component

The most important component. Experienced developers live in code blocks.

**HTML Pattern:**
```html
<!-- Basic command block -->
<div class="code-block" data-lang="bash">
  <div class="code-block__header">
    <span class="code-block__lang">bash</span>
    <button class="code-block__copy" aria-label="Copy to clipboard">
      <svg class="icon icon--copy" aria-hidden="true"><!-- clipboard SVG --></svg>
      <svg class="icon icon--check icon--hidden" aria-hidden="true"><!-- checkmark SVG --></svg>
      <span class="code-block__copy-label">Copy</span>
    </button>
  </div>
  <pre class="code-block__pre"><code class="language-bash">gh copilot suggest "list all running docker containers"</code></pre>
</div>

<!-- File example block (YAML/Markdown with filepath) -->
<div class="code-block code-block--file" data-lang="yaml">
  <div class="code-block__header">
    <span class="code-block__filepath">
      <svg class="icon icon--file" aria-hidden="true"><!-- file icon --></svg>
      .github/agents/triage-bot.md
    </span>
    <button class="code-block__copy" aria-label="Copy to clipboard">
      <!-- copy button same as above -->
    </button>
  </div>
  <pre class="code-block__pre"><code class="language-yaml">---
name: triage-bot
description: Triages GitHub issues and suggests labels
---</code></pre>
</div>
```

**CSS Pattern:**
```css
.code-block {
  margin: var(--spacing-lg) 0;
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);
  overflow: hidden;
  background: var(--color-bg-overlay);
}

.code-block__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--spacing-xs) var(--spacing-md);
  background: var(--color-bg-subtle);
  border-bottom: 1px solid var(--color-border);
}

.code-block__lang {
  font-family: var(--font-mono);
  font-size: var(--font-size-sm);
  color: var(--color-text-secondary);
  text-transform: lowercase;
}

.code-block__filepath {
  display: flex;
  align-items: center;
  gap: var(--spacing-xs);
  font-family: var(--font-mono);
  font-size: var(--font-size-sm);
  color: var(--color-text-link);        /* filepath = link color = distinctive */
}

.code-block__copy {
  display: flex;
  align-items: center;
  gap: var(--spacing-xs);
  padding: 2px var(--spacing-sm);
  background: none;
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);
  color: var(--color-text-secondary);
  font-size: var(--font-size-sm);
  cursor: pointer;
  transition: color 0.15s, border-color 0.15s;
}

.code-block__copy:hover {
  color: var(--color-text-primary);
  border-color: var(--color-border-subtle);
}

.code-block__copy--copied {
  color: var(--color-success);
  border-color: var(--color-success);
}

.code-block__pre {
  margin: 0;
  padding: var(--spacing-md) var(--spacing-lg);
  overflow-x: auto;
  font-family: var(--font-mono);
  font-size: var(--font-size-sm);
  line-height: 1.6;
}
```

**JS Pattern:**
```javascript
function initCopyButtons() {
  // Use event delegation — catches all current and future copy buttons
  document.addEventListener('click', async (e) => {
    const btn = e.target.closest('.code-block__copy');
    if (!btn) return;

    const codeBlock = btn.closest('.code-block');
    const code = codeBlock.querySelector('code').textContent;

    try {
      await navigator.clipboard.writeText(code);
    } catch {
      // Fallback for older browsers
      const ta = document.createElement('textarea');
      ta.value = code;
      ta.style.position = 'fixed';
      ta.style.opacity = '0';
      document.body.appendChild(ta);
      ta.select();
      document.execCommand('copy');
      document.body.removeChild(ta);
    }

    // Visual feedback
    const label = btn.querySelector('.code-block__copy-label');
    const iconCopy = btn.querySelector('.icon--copy');
    const iconCheck = btn.querySelector('.icon--check');

    btn.classList.add('code-block__copy--copied');
    if (label) label.textContent = 'Copied!';
    if (iconCopy) iconCopy.classList.add('icon--hidden');
    if (iconCheck) iconCheck.classList.remove('icon--hidden');

    setTimeout(() => {
      btn.classList.remove('code-block__copy--copied');
      if (label) label.textContent = 'Copy';
      if (iconCopy) iconCopy.classList.remove('icon--hidden');
      if (iconCheck) iconCheck.classList.add('icon--hidden');
    }, 2000);
  });
}
```

---

### 3. Collapsible Subsection

**Use native `<details>`/`<summary>`** — accessible by default, no JS required to function, CSS-only animation achievable.

```html
<details class="collapsible" id="agent-frontmatter-reference">
  <summary class="collapsible__trigger">
    <svg class="collapsible__chevron" aria-hidden="true"><!-- chevron-right SVG --></svg>
    <span>Agent Frontmatter Reference</span>
    <span class="collapsible__badge">reference</span>
  </summary>
  <div class="collapsible__body">
    <!-- content here; can contain code-blocks, tables, prose -->
  </div>
</details>
```

**CSS Pattern:**
```css
.collapsible {
  margin: var(--spacing-md) 0;
  border: 1px solid var(--color-border-muted);
  border-radius: var(--border-radius);
  background: var(--color-bg-surface);
  overflow: hidden;
}

.collapsible__trigger {
  display: flex;
  align-items: center;
  gap: var(--spacing-sm);
  padding: var(--spacing-sm) var(--spacing-md);
  cursor: pointer;
  list-style: none;               /* remove default marker */
  color: var(--color-text-primary);
  font-weight: 500;
  user-select: none;
}

.collapsible__trigger::-webkit-details-marker { display: none; } /* Safari */

.collapsible__trigger:hover {
  background: var(--color-bg-subtle);
}

.collapsible__chevron {
  width: 16px;
  height: 16px;
  color: var(--color-text-secondary);
  transition: transform 0.2s ease;
  flex-shrink: 0;
}

details[open] .collapsible__chevron {
  transform: rotate(90deg);
}

.collapsible__badge {
  margin-left: auto;
  padding: 1px 6px;
  background: var(--color-bg-muted);
  border-radius: 10px;
  font-size: 11px;
  color: var(--color-text-secondary);
  font-weight: 400;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.collapsible__body {
  padding: var(--spacing-md) var(--spacing-lg);
  border-top: 1px solid var(--color-border-muted);
}
```

**JS for smooth animation** (optional, progressive enhancement):
```javascript
function initCollapsibles() {
  // Smooth open/close animation using the `toggle` event
  document.addEventListener('toggle', (e) => {
    if (!e.target.classList.contains('collapsible')) return;

    const body = e.target.querySelector('.collapsible__body');
    if (!body) return;

    if (e.target.open) {
      body.style.maxHeight = body.scrollHeight + 'px';
    } else {
      body.style.maxHeight = '0';
    }
  }, true);
}
```

---

### 4. Sidebar Scrollspy

Track active section as user scrolls; highlight matching sidebar link.

```javascript
function initScrollspy() {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      const id = entry.target.id;
      const link = document.querySelector(`.sidebar-nav__link[href="#${id}"]`);
      if (!link) return;

      if (entry.isIntersecting) {
        // Remove active from all links in the active sidebar
        const activeSidebar = document.querySelector('.sidebar-nav:not(.sidebar-nav--hidden)');
        activeSidebar?.querySelectorAll('.sidebar-nav__link--active')
          .forEach(l => l.classList.remove('sidebar-nav__link--active'));

        link.classList.add('sidebar-nav__link--active');

        // Scroll link into sidebar view if needed
        link.scrollIntoView({ block: 'nearest', behavior: 'smooth' });
      }
    });
  }, {
    rootMargin: '-10% 0px -80% 0px',  /* trigger when section is in top 10-20% of viewport */
    threshold: 0
  });

  document.querySelectorAll('.content-section[id]').forEach(section => {
    observer.observe(section);
  });
}
```

---

### 5. Syntax Highlighting

**Recommendation: Embed Prism.js inline (minified).**

Prism.js core + the 4 languages needed (bash, yaml, markdown, json) compresses to ~18KB unminified, ~8KB gzipped — acceptable for a single-file document.

**Why Prism over Highlight.js:**
- Smaller core, more modular language loading
- Better YAML support (agent/skill frontmatter is YAML)
- Shell/bash plugin handles `$` prompts and command flags well
- Active maintenance as of 2024

**Embed strategy:**
```html
<!-- In <head>, after the main <style> block -->
<style id="prism-theme">
  /* Custom Prism theme using the color tokens above */
  code[class*="language-"],
  pre[class*="language-"] {
    color: var(--color-text-code);
    background: none;
    font-family: var(--font-mono);
    font-size: var(--font-size-sm);
  }
  .token.keyword    { color: var(--syn-keyword); }
  .token.string     { color: var(--syn-string); }
  .token.comment    { color: var(--syn-comment); font-style: italic; }
  .token.function   { color: var(--syn-function); }
  .token.number     { color: var(--syn-number); }
  .token.operator   { color: var(--syn-operator); }
  .token.property   { color: var(--syn-property); }
  .token.punctuation{ color: var(--syn-punctuation); }
  /* bash-specific: $ prompt */
  .token.command    { color: var(--color-accent); }
</style>

<!-- At bottom of <body>, just before the app JS -->
<script>
/* Prism.js vX.XX.X — core + bash + yaml + json + markdown — minified inline */
/* Obtain from: https://prismjs.com/download.html */
/* Select: Minify + bash, yaml, markdown, json */
</script>
```

**Initialization:** Prism auto-highlights all `<code class="language-*">` elements on `DOMContentLoaded` by default. No additional init call needed.

---

### 6. Callout / Note Boxes

Important for an experienced-dev audience that scans quickly.

```html
<div class="callout callout--info">
  <svg class="callout__icon" aria-hidden="true"><!-- info icon --></svg>
  <div class="callout__content">
    <strong class="callout__title">Note</strong>
    <p>Agents run in the context of your repository's default branch...</p>
  </div>
</div>

<!-- Variants: callout--warning, callout--danger, callout--tip -->
```

```css
.callout {
  display: flex;
  gap: var(--spacing-md);
  padding: var(--spacing-md);
  border-radius: var(--border-radius);
  border-left: 3px solid;
  margin: var(--spacing-lg) 0;
}

.callout--info    { background: #0c2a4a; border-color: var(--color-info); }
.callout--warning { background: #2e2000; border-color: var(--color-warning); }
.callout--danger  { background: #2d1216; border-color: var(--color-danger); }
.callout--tip     { background: #0e2918; border-color: var(--color-success); }
```

---

## Information Architecture for Scannability

Target audience is experienced developers who **scan, not read**. Structure accordingly:

### Content Hierarchy Rules

| Level | Element | Purpose |
|-------|---------|---------|
| L1 | Tab label (CLI Intro / Agents / Skills) | "Where am I?" |
| L2 | `<h2>` section heading + sidebar link | "What's in this section?" |
| L3 | `<h3>` subsection or collapsible trigger | "What's this detail?" |
| L4 | Code block with filepath label | "Show me the real thing" |
| L5 | Callout boxes | "What do I need to know?" |

### Scannability Patterns

**Narrative + code pairing:** Every prose paragraph should be immediately followed by a code example. Never more than 3 sentences before a code block.

**Section structure template:**
```
<section id="...">
  <h2>Heading</h2>
  <p>One-sentence what + why</p>
  [code-block: minimal working example]
  <p>Two sentences of explanation</p>
  [code-block: real-world example with filepath]
  <details class="collapsible">Deep reference (collapsed by default)</details>
</section>
```

**Collapsible discipline:** Default collapsed = reference material. Default open = narrative flow content. Never collapse the first code block in a section.

**Callout placement:** Use callouts for gotchas that apply to the entire section. Inline `<code>` for specific technical terms.

---

## Suggested Build Order

Build in this sequence to progressively validate each layer before adding complexity:

### Stage 1: Base Layout
- CSS custom properties (all color tokens)
- Grid shell (header / sidebar / main)
- `<header>` and `<aside>` structure with placeholder content
- Verify: layout holds at desktop + mobile

### Stage 2: Theme + Typography
- All typography tokens (font stacks, sizes, line heights)
- Base element styles (h1-h6, p, a, ul, table)
- Scrollbar styling
- Verify: prose content looks polished before any components

### Stage 3: Navigation Components
- Tab nav CSS + JS (show/hide panels)
- Sidebar nav CSS (links, groups, active states)
- Scrollspy JS (Intersection Observer)
- Verify: all three tabs switch correctly, sidebar highlights

### Stage 4: Code Block Component
- Code block HTML structure
- Code block CSS (header, filepath, copy button)
- Copy button JS (Clipboard API + fallback)
- Embed Prism.js (minified, inline)
- Prism theme CSS (maps token classes → color tokens)
- Verify: code blocks look correct for bash, yaml, markdown, json

### Stage 5: Collapsible Component
- `<details>`/`<summary>` CSS (chevron rotation, body padding)
- Optional smooth-animation JS
- Badge variant CSS
- Verify: open/close works, animation is smooth

### Stage 6: Callout Component
- Callout CSS (4 variants: info, warning, danger, tip)
- Verify: all variants render correctly with icons

### Stage 7: Content Population
- CLI Intro article: overview → suggest → explain → alias
- Agents article: concepts → create (AI-assisted) → manual → orchestration (with `.github/agents/*.md` example)
- Skills article: concepts → create → `.github/skills/*.md` example → invocation
- Verify: all content accurate, all code blocks syntax-highlighted, all collapsibles populated

### Stage 8: Polish
- Smooth scroll behavior
- Mobile nav (hamburger toggle for sidebar)
- Keyboard navigation (tab order, focus styles)
- Print styles (optional)
- Final QA: copy buttons, all tabs, all collapsibles

---

## Anti-Patterns to Avoid

### Anti-Pattern 1: Framework Creep
**What:** Starting with "just React" or "just Alpine.js" because it's easier initially.
**Why bad:** Violates the single-file no-dependency constraint. The interactivity here (tabs, collapsibles, copy buttons) is 50 lines of vanilla JS.
**Instead:** Vanilla JS with event delegation. No build step needed.

### Anti-Pattern 2: Overusing Tabs
**What:** Using tabs inside sections to show/hide content variants.
**Why bad:** Nested tabs create disorientation ("which tabs am I in?"). Experienced devs hate hunt-and-peck navigation.
**Instead:** Show both variants with clear labels, or use a collapsible for the secondary variant.

### Anti-Pattern 3: Inline Styles for Theming
**What:** Hardcoding `color: #3fb950` directly on elements.
**Why bad:** Single-file doc is hard enough to maintain; scattered magic strings make theme changes require global search-replace.
**Instead:** All colors as CSS custom properties on `:root`. Components reference tokens only.

### Anti-Pattern 4: Long Prose Before Code
**What:** Three paragraphs of explanation before the first example.
**Why bad:** This audience reads code first, prose second. Long prose before code signals "skip this section."
**Instead:** One sentence of context → code → explanation.

### Anti-Pattern 5: Monolithic `<script>` Block
**What:** All JS in one undifferentiated block.
**Why bad:** Impossible to maintain; initialization order issues creep in.
**Instead:** Separate named functions per component, each with `init*()` prefix, called from a single `document.addEventListener('DOMContentLoaded', () => { initTabs(); initCopyButtons(); initCollapsibles(); initScrollspy(); })`.

### Anti-Pattern 6: Un-anchored Sections
**What:** `<section>` without an `id` attribute.
**Why bad:** Sidebar nav links and scrollspy both depend on anchors. Missing IDs silently break navigation.
**Instead:** Every `<section>` and major heading gets an `id` in kebab-case matching the sidebar link's `href`.

---

## Scalability Considerations

This is a single self-contained file — "scalability" means file size and maintainability:

| Concern | Guidance |
|---------|----------|
| File size | Target under 200KB uncompressed. Prism.js inline adds ~25KB. Budget ~150KB for content + CSS + JS. |
| Adding new sections | Add `<section id="...">` inside the relevant `<article>`, add a `<li>` to the corresponding sidebar nav, both follow established patterns. |
| Updating theme colors | Change values in `:root` only — nothing else needs to change. |
| Multiple code languages | Prism supports adding more language definitions inline; add as needed. |
| Link sharing | Tab state not in URL by default. Use `location.hash` to persist active tab if shareable deep links matter. |

---

## Sources

- HTML ARIA tab pattern: https://www.w3.org/WAI/ARIA/apg/patterns/tabs/ (HIGH confidence)
- Prism.js documentation: https://prismjs.com/ (HIGH confidence)
- GitHub Primer design tokens: https://primer.style/primitives/colors (HIGH confidence — palette derived from)
- Intersection Observer API: https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API (HIGH confidence)
- `<details>`/`<summary>` element: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details (HIGH confidence)
- Clipboard API: https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API (HIGH confidence)

I'm studying **{{SUBJECT_NAME}}** at university and I want you to prepare a comprehensive, exam-ready study guide as a new page in my existing Jekyll-based guide site. Build it iteratively in the phases described below — do not skip ahead, wait for my confirmation between major phases.

### Materials I'm providing
- Main textbook: `{{TEXTBOOK_FILE}}`
- Lab assignments (if any): `{{LAB_FILES}}`
- Exam questions / study topics (if any): `{{EXAM_FILE}}`
- Language of the source materials: **{{LANGUAGE}}** (produce the guide in this same language)
- Reference — my existing guide (as a style/structure example): `{{EXISTING_GUIDE_FILE}}`

### Output format
A **single Jekyll page** (one `.html` file) that plugs into my existing site. The file must start with YAML front matter:
```yaml
---
layout: guide
title: "{{SUBJECT_NAME}}"
---
```
Followed by the page body: the sidebar `<nav>` + the `<main>` content + the scroll-spy `<script>`. No `<html>`, `<head>`, `<body>`, `<style>`, or `<link>` tags — those live in the shared layout. No inline CSS except unavoidable chapter-color overrides — use only the component classes already defined in the shared stylesheet.

Save the file to `/mnt/user-data/outputs/` and use `present_files` to share it.

### The shared component library (already defined — just use the classes)

- `.sidebar` with `.sidebar-logo`, `.nav-item`, `.nav-dot` — fixed left nav with scroll-spy
- `.sidebar-toggle` — mobile hamburger button
- `.main` with `.hero` and `.chapter` sections
- `.chapter-header` with `.chapter-num` badge and `.chapter-title`
- `.concept` cards with `.concept-title` (prefixed by `⬡` hexagon) — the main content block
- `.comparison` tables — clean borderless with uppercase monospace headers
- `.proto-grid` with `.proto-card` — responsive card grid for listing items
- `.ip-visual` — monospace dark box for worked examples, formulas, step-by-step flows
- `.frame-diagram` with `.frame-field` — horizontal colored boxes for packet/header layouts
- `.osi-stack` with `.osi-layer` — vertical stacked layers for layered models
- `.note-block.important` (blue) — key insights, must-know facts
- `.note-block.exam` (purple) — formulas, mnemonics, exam tips
- `<span class="key-term">` — inline emphasis for key terms
- `.glossary-grid` with `.abbr-card` containing `.abbr-key`, `.abbr-full`, `.abbr-desc` — for the glossary
- `.glossary-category` — section divider inside the glossary

CSS variables available: `--bg`, `--surface`, `--surface2`, `--border`, `--text`, `--text-dim`, `--accent`, `--accent2` through `--accent6`, `--code-bg`, and `--chapter-1` through `--chapter-10`+ (one per chapter).

Color-code each chapter: the `.chapter-num` badge background, the `.concept-title` color, the `.nav-dot` in the sidebar — all use the same `--chapter-N` variable for that chapter.

### Page structure (mirror this exactly)

```
[YAML front matter]

<button class="sidebar-toggle" onclick="toggleSidebar()">&#9776;</button>

<nav class="sidebar" id="sidebar">
  <div class="sidebar-logo">
    <h1><a href="{{ '/' | relative_url }}">{{SHORT_TITLE}}</a></h1>
    <p>{{SUBJECT_NAME}} · {{INSTITUTION}}</p>
  </div>
  <div class="nav-item active" data-target="hero" onclick="scrollTo('hero')">
    <div class="nav-dot" style="background:var(--text-dim)"></div> Огляд / Overview
  </div>
  [one .nav-item per chapter]
  [one .nav-item for glossary]
</nav>

<main class="main" id="main">
  <section class="hero" id="hero">
    <h1>...</h1>
    <p class="subtitle">...</p>
    <div class="meta">
      <span>N розділів</span>
      [a few topical badges]
    </div>
  </section>

  [one <section class="chapter" id="chN"> per chapter, with concept cards]

  <section class="chapter" id="glossary">
    [glossary with categorized abbr-cards]
  </section>
</main>

<footer class="footer">
  <p>[source attribution] · <a href="{{ '/' | relative_url }}">← Всі посібники / All guides</a></p>
</footer>

<script>
  [scrollTo, toggleSidebar, IntersectionObserver for scroll-spy]
</script>
```

Use `{{ '/' | relative_url }}` (literal Jekyll Liquid syntax — output exactly as written, do not substitute) for links back to the hub page.

### Content principles

1. **Distill, don't reproduce.** Extract essential concepts, definitions, and relationships. A good study guide is 10–20% the length of the source but covers 100% of what's testable.
2. **Every claim earns its place.** If it's not needed to understand a concept or answer an exam question, cut it.
3. **Prefer tables over prose** for comparisons (X vs Y), classifications, and anything with parallel structure — use `.comparison`.
4. **Prefer visuals over text** for processes, structures, and hierarchies — use `.ip-visual`, `.frame-diagram`, or `.osi-stack`.
5. **Worked examples matter.** For any calculation, formula, or procedure, show one concrete worked example inside an `.ip-visual` block.
6. **Write in the source language.** All content in `{{LANGUAGE}}` except universally-known technical abbreviations (which go in the glossary with full English expansions).
7. **No inline styles except unavoidable ones** — e.g. chapter-color overrides on `.concept-title`, `.chapter-num`, or `.frame-field` backgrounds. Everything else relies on the shared stylesheet.

### Execution phases

**Phase 1 — Recon.** Read the textbook's table of contents and the first 10–20 pages. Also skim the reference guide I provided to internalize the structure and style. Report back: the chapter list you'll build, color assignments per chapter (`--chapter-1` through `--chapter-N`), any open questions. Wait for my confirmation.

**Phase 2 — First draft.** Produce the initial Jekyll page with front matter, sidebar nav, hero, and all chapters with core concept blocks. Cover main topics but don't over-detail — aim for 4–8 concept blocks per chapter. Share the file.

**Phase 3 — Gap review.** Systematically cross-reference the guide against the full textbook content, chapter by chapter, section by section. Present a gap list — what's missing, thin, or wrong — before fixing anything. Wait for my confirmation.

**Phase 4 — Fill gaps.** Apply targeted edits using `str_replace` to add missing content. Don't rewrite working sections. Validate HTML tag balance after edits.

**Phase 5 — Enrich from labs and exam questions** (if provided). Extract practical concepts (tools, commands, procedures, worked examples) and exam-relevant framings. Add to existing chapters rather than creating new ones. Tag lab-derived content with "(Lab N)" in the concept title.

**Phase 6 — Detail expansion.** Review each section again for concepts that are mentioned but not explained. Header structures → show all fields with bit sizes. Protocols → show the full handshake or flow. Algorithms → show a worked example. Comparisons → make them tabular.

**Phase 7 — Glossary.** Add a final `#glossary` section at the end with a sidebar link. Extract every abbreviation used anywhere in the guide. Present as `.abbr-card`s grouped by `.glossary-category`. Each card: acronym (`.abbr-key`), full expansion (`.abbr-full`, italic), one-line description (`.abbr-desc`). Aim for completeness — don't leave any unexplained acronym.

### Quality bar — the guide is done when:
- A student unfamiliar with the subject can read it top to bottom and understand the field
- Every exam question the user provides can be answered using only content from the guide
- Every abbreviation used in the body is defined in the glossary
- Every formula has a worked example
- Every multi-step process has a visual representation
- HTML validates (balanced tags, matching sections)
- Navigation, scroll-spy, and mobile sidebar toggle all work
- No inline `<style>` blocks or external CSS links — all styling through shared classes

### What to ask me before starting
After reading the materials, check in on:
- Whether any chapters should be split, merged, or reordered
- Whether topics outside the textbook (lectures, slides) should be covered
- Whether the exam format (multiple choice, open questions, practical) should shape emphasis
- The `{{SHORT_TITLE}}` for the sidebar logo and `{{INSTITUTION}}` attribution

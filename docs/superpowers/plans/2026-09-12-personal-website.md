# Personal Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a minimal, clean two-page personal academic website deployable on GitHub Pages with no build step.

**Architecture:** Two static HTML pages (`index.html`, `publications.html`) share a single `style.css`. Both fetch `data/publications.json` at page load and render publication lists via vanilla JavaScript. No frameworks, no build tools, no dependencies.

**Tech Stack:** HTML5, CSS3, vanilla JavaScript (ES6 fetch + DOM), GitHub Pages

## Global Constraints

- No external CSS frameworks or JS libraries — zero dependencies
- No build step — files must be serveable as-is from GitHub Pages
- `max-width: 700px` centered column, `2rem` side padding
- Font: `-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`
- Colors: background `#fff`, text `#111`, links `#555`
- No images, no decorative elements, no color accents
- Publications data lives exclusively in `data/publications.json` — HTML never hardcodes paper entries

---

## File Map

| File | Action | Responsibility |
|------|--------|----------------|
| `style.css` | Create | All shared styles: reset, layout, nav, typography, publication entries, experience |
| `data/publications.json` | Create | Single source of truth for all publication data |
| `index.html` | Create | Main page: nav, intro, selected publications (JS-rendered), experience |
| `publications.html` | Create | All publications subpage: nav, three typed sections (JS-rendered) |

---

### Task 1: Stylesheet (`style.css`)

**Files:**
- Create: `style.css`

**Interfaces:**
- Produces: CSS classes consumed by both HTML files:
  - `body` — base font, color, background
  - `.container` — centered column, max-width 700px, padding
  - `nav` — flex row, space-between, border-bottom
  - `.nav-name` — left nav text
  - `.nav-link` — right nav link
  - `h1` — page title size
  - `.subtitle` — role/affiliation line under h1
  - `.social-links` — horizontal link list, gap between items
  - `section` — top margin separator
  - `h2` — section heading style
  - `.pub-list` — unstyled list for publication entries
  - `.pub-entry` — individual publication block, bottom margin
  - `.pub-title a` — publication title link style
  - `.pub-meta` — authors + venue/year line, muted color
  - `.all-pubs-link` — "All publications →" link style
  - `.exp-list` — unstyled list for experience entries
  - `.exp-entry` — individual experience block
  - `.exp-role` — bold role + company
  - `.exp-date` — muted date range
  - `.hidden` — `display: none`

- [ ] **Step 1: Create `style.css` with all required styles**

```css
/* style.css */
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  font-size: 16px;
  line-height: 1.6;
  color: #111;
  background: #fff;
}

.container {
  max-width: 700px;
  margin: 0 auto;
  padding: 0 2rem;
}

/* Nav */
nav {
  padding: 1.5rem 0;
  border-bottom: 1px solid #e8e8e8;
  margin-bottom: 3rem;
}

nav .container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.nav-name {
  font-weight: 600;
  color: #111;
  text-decoration: none;
}

.nav-link {
  color: #555;
  text-decoration: none;
  font-size: 0.95rem;
}

.nav-link:hover {
  color: #111;
}

/* Intro */
h1 {
  font-size: 1.75rem;
  font-weight: 700;
  margin-bottom: 0.35rem;
  letter-spacing: -0.01em;
}

.subtitle {
  color: #555;
  font-size: 0.95rem;
  margin-bottom: 1rem;
}

.bio {
  margin-bottom: 1.25rem;
  color: #222;
}

.social-links {
  display: flex;
  gap: 1.25rem;
  list-style: none;
  flex-wrap: wrap;
}

.social-links a {
  color: #555;
  text-decoration: none;
  font-size: 0.9rem;
}

.social-links a:hover {
  color: #111;
}

/* Sections */
section {
  margin-top: 3rem;
  margin-bottom: 1rem;
}

h2 {
  font-size: 1.1rem;
  font-weight: 600;
  letter-spacing: 0.03em;
  text-transform: uppercase;
  margin-bottom: 1.25rem;
  color: #111;
}

/* Publications */
.pub-list {
  list-style: none;
}

.pub-entry {
  margin-bottom: 1.5rem;
}

.pub-title a {
  color: #111;
  text-decoration: none;
  font-weight: 500;
}

.pub-title a:hover {
  color: #555;
}

.pub-meta {
  font-size: 0.88rem;
  color: #555;
  margin-top: 0.15rem;
}

.all-pubs-link {
  display: inline-block;
  margin-top: 0.5rem;
  color: #555;
  text-decoration: none;
  font-size: 0.9rem;
}

.all-pubs-link:hover {
  color: #111;
}

/* Experience */
.exp-list {
  list-style: none;
}

.exp-entry {
  margin-bottom: 1.25rem;
}

.exp-role {
  font-weight: 500;
  color: #111;
}

.exp-date {
  font-size: 0.88rem;
  color: #555;
  margin-top: 0.1rem;
}

.exp-desc {
  font-size: 0.9rem;
  color: #444;
  margin-top: 0.15rem;
}

/* Utility */
.hidden {
  display: none;
}

/* Footer */
footer {
  margin-top: 4rem;
  padding: 2rem 0;
  border-top: 1px solid #e8e8e8;
  font-size: 0.85rem;
  color: #aaa;
}
```

- [ ] **Step 2: Verify file exists**

```bash
ls -la style.css
```
Expected: file listed with non-zero size.

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "feat: add shared stylesheet"
```

---

### Task 2: Publications data (`data/publications.json`)

**Files:**
- Create: `data/publications.json`

**Interfaces:**
- Produces: JSON array at key `publications`, each object with fields:
  `title` (string), `authors` (string), `venue` (string), `year` (number),
  `type` ("journal"|"conference"|"workshop"), `url` (string), `selected` (boolean)
- Consumed by: `index.html` (filters `selected === true`), `publications.html` (groups by `type`)

- [ ] **Step 1: Create `data/` directory and `publications.json` with placeholder entries**

```bash
mkdir -p data
```

Then create `data/publications.json`:

```json
{
  "publications": [
    {
      "title": "Placeholder: Replace with your paper title",
      "authors": "L. Kirchdorfer, A. Coauthor",
      "venue": "NeurIPS",
      "year": 2024,
      "type": "conference",
      "url": "https://arxiv.org/abs/0000.00001",
      "selected": true
    },
    {
      "title": "Placeholder: Replace with your paper title",
      "authors": "L. Kirchdorfer, B. Coauthor, C. Coauthor",
      "venue": "ICML",
      "year": 2023,
      "type": "conference",
      "url": "https://arxiv.org/abs/0000.00002",
      "selected": true
    },
    {
      "title": "Placeholder: Replace with your paper title",
      "authors": "L. Kirchdorfer, D. Coauthor",
      "venue": "Journal of Machine Learning Research",
      "year": 2022,
      "type": "journal",
      "url": "https://arxiv.org/abs/0000.00003",
      "selected": false
    },
    {
      "title": "Placeholder: Replace with your paper title",
      "authors": "L. Kirchdorfer, E. Coauthor",
      "venue": "NeurIPS Workshop on XYZ",
      "year": 2023,
      "type": "workshop",
      "url": "https://arxiv.org/abs/0000.00004",
      "selected": false
    }
  ]
}
```

- [ ] **Step 2: Validate JSON is well-formed**

```bash
python3 -c "import json; json.load(open('data/publications.json')); print('valid')"
```
Expected output: `valid`

- [ ] **Step 3: Commit**

```bash
git add data/publications.json
git commit -m "feat: add publications data with placeholder entries"
```

---

### Task 3: Main page (`index.html`)

**Files:**
- Create: `index.html`

**Interfaces:**
- Consumes:
  - `style.css` — all CSS classes defined in Task 1
  - `data/publications.json` — fetched via `fetch('./data/publications.json')`, renders entries where `selected === true`, sorted by `year` descending
- Produces: browsable `index.html` at repo root

- [ ] **Step 1: Create `index.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Lukas Kirchdorfer</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <nav>
    <div class="container">
      <a href="index.html" class="nav-name">Lukas Kirchdorfer</a>
      <a href="publications.html" class="nav-link">Publications</a>
    </div>
  </nav>

  <main class="container">

    <!-- Intro -->
    <section id="intro">
      <h1>Lukas Kirchdorfer</h1>
      <p class="subtitle">Senior AI Scientist at SAP &nbsp;·&nbsp; PhD in Artificial Intelligence</p>
      <p class="bio">
        I work on machine learning research at SAP, focusing on [your research focus here].
        My work spans [research area 1] and [research area 2].
        Previously, I completed my PhD at [university].
      </p>
      <ul class="social-links">
        <li><a href="https://scholar.google.de/citations?hl=de&user=4NuUVSkAAAAJ" target="_blank" rel="noopener">Google Scholar</a></li>
        <li><a href="https://github.com/lukaskirchdorfer" target="_blank" rel="noopener">GitHub</a></li>
        <li><a href="https://www.linkedin.com/in/lukas-kirchdorfer/" target="_blank" rel="noopener">LinkedIn</a></li>
      </ul>
    </section>

    <!-- Selected Publications -->
    <section id="publications">
      <h2>Selected Publications</h2>
      <ul class="pub-list" id="selected-pub-list">
        <!-- Rendered by JavaScript -->
      </ul>
      <a href="publications.html" class="all-pubs-link">All publications →</a>
    </section>

    <!-- Experience -->
    <section id="experience">
      <h2>Experience</h2>
      <ul class="exp-list">
        <li class="exp-entry">
          <div class="exp-role">Senior AI Scientist &nbsp;·&nbsp; SAP</div>
          <div class="exp-date">2022 – Present</div>
        </li>
        <li class="exp-entry">
          <div class="exp-role">PhD Researcher &nbsp;·&nbsp; [University Name]</div>
          <div class="exp-date">2018 – 2022</div>
        </li>
        <li class="exp-entry">
          <div class="exp-role">[Previous Role] &nbsp;·&nbsp; [Company]</div>
          <div class="exp-date">[Start] – [End]</div>
        </li>
      </ul>
    </section>

  </main>

  <footer>
    <div class="container">© 2026 Lukas Kirchdorfer</div>
  </footer>

  <script>
    async function loadSelectedPublications() {
      const list = document.getElementById('selected-pub-list');
      try {
        const response = await fetch('./data/publications.json');
        const data = await response.json();
        const selected = data.publications
          .filter(p => p.selected === true)
          .sort((a, b) => b.year - a.year);
        selected.forEach(pub => {
          const li = document.createElement('li');
          li.className = 'pub-entry';
          li.innerHTML = `
            <div class="pub-title"><a href="${pub.url}" target="_blank" rel="noopener">${pub.title}</a></div>
            <div class="pub-meta">${pub.authors} &nbsp;·&nbsp; ${pub.venue}, ${pub.year}</div>
          `;
          list.appendChild(li);
        });
      } catch (e) {
        list.innerHTML = '<li>Publications unavailable.</li>';
      }
    }
    loadSelectedPublications();
  </script>
</body>
</html>
```

- [ ] **Step 2: Open in browser and verify layout**

```bash
open index.html
```

Check:
- Nav shows "Lukas Kirchdorfer" left, "Publications" right
- Intro section renders with name, subtitle, bio, three social links
- Selected publications list populated from JSON (placeholder titles visible)
- Experience section shows hardcoded entries
- Footer visible

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add main page with intro, selected publications, experience"
```

---

### Task 4: Publications subpage (`publications.html`)

**Files:**
- Create: `publications.html`

**Interfaces:**
- Consumes:
  - `style.css` — all CSS classes from Task 1
  - `data/publications.json` — fetched via `fetch('./data/publications.json')`, groups by `type`, sorts each group by `year` descending
- Produces: browsable `publications.html` listing all papers in three sections

- [ ] **Step 1: Create `publications.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Publications – Lukas Kirchdorfer</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <nav>
    <div class="container">
      <a href="index.html" class="nav-name">Lukas Kirchdorfer</a>
      <a href="index.html" class="nav-link">Home</a>
    </div>
  </nav>

  <main class="container">

    <!-- Journals -->
    <section id="journals" class="hidden">
      <h2>Journals</h2>
      <ul class="pub-list" id="journal-list"></ul>
    </section>

    <!-- Conferences -->
    <section id="conferences" class="hidden">
      <h2>Conferences</h2>
      <ul class="pub-list" id="conference-list"></ul>
    </section>

    <!-- Workshops -->
    <section id="workshops" class="hidden">
      <h2>Workshops</h2>
      <ul class="pub-list" id="workshop-list"></ul>
    </section>

  </main>

  <footer>
    <div class="container">© 2026 Lukas Kirchdorfer</div>
  </footer>

  <script>
    function renderPubEntry(pub) {
      const li = document.createElement('li');
      li.className = 'pub-entry';
      li.innerHTML = `
        <div class="pub-title"><a href="${pub.url}" target="_blank" rel="noopener">${pub.title}</a></div>
        <div class="pub-meta">${pub.authors} &nbsp;·&nbsp; ${pub.venue}, ${pub.year}</div>
      `;
      return li;
    }

    function populateSection(sectionId, listId, pubs) {
      if (pubs.length === 0) return;
      const section = document.getElementById(sectionId);
      const list = document.getElementById(listId);
      section.classList.remove('hidden');
      pubs
        .sort((a, b) => b.year - a.year)
        .forEach(pub => list.appendChild(renderPubEntry(pub)));
    }

    async function loadAllPublications() {
      try {
        const response = await fetch('./data/publications.json');
        const data = await response.json();
        const journals = data.publications.filter(p => p.type === 'journal');
        const conferences = data.publications.filter(p => p.type === 'conference');
        const workshops = data.publications.filter(p => p.type === 'workshop');
        populateSection('journals', 'journal-list', journals);
        populateSection('conferences', 'conference-list', conferences);
        populateSection('workshops', 'workshop-list', workshops);
      } catch (e) {
        document.querySelector('main').innerHTML += '<p>Publications unavailable.</p>';
      }
    }
    loadAllPublications();
  </script>
</body>
</html>
```

- [ ] **Step 2: Open in browser and verify layout**

```bash
open publications.html
```

Check:
- Nav shows "Lukas Kirchdorfer" left, "Home" right
- Journals section visible with 1 placeholder entry
- Conferences section visible with 2 placeholder entries
- Workshops section visible with 1 placeholder entry
- Sections with zero entries would be hidden (all four types represented in JSON, so all visible)

- [ ] **Step 3: Verify "Home" nav link returns to index**

Click "Home" in nav → should navigate to `index.html`.

- [ ] **Step 4: Commit**

```bash
git add publications.html
git commit -m "feat: add publications subpage with journal/conference/workshop sections"
```

---

### Task 5: Wire up and deploy

**Files:**
- Modify: `README.md`

**Interfaces:**
- Consumes: all files from Tasks 1–4
- Produces: live site at `https://lukas-kirchdorfer.github.io`

- [ ] **Step 1: Do a full local review — open both pages**

```bash
open index.html
open publications.html
```

Verify checklist:
- [ ] Nav links work on both pages (cross-navigation)
- [ ] "All publications →" on homepage links to `publications.html`
- [ ] All three social links open correct URLs
- [ ] Publications render from JSON on both pages
- [ ] Empty sections are hidden on publications page
- [ ] No console errors (open browser DevTools → Console)

- [ ] **Step 2: Update README**

Replace contents of `README.md`:

```markdown
# lukas-kirchdorfer.github.io

Personal website of Lukas Kirchdorfer — Senior AI Scientist at SAP.

Live at: https://lukas-kirchdorfer.github.io

## Adding publications

Edit `data/publications.json`. Each entry:

```json
{
  "title": "Paper Title",
  "authors": "L. Kirchdorfer, ...",
  "venue": "NeurIPS",
  "year": 2024,
  "type": "conference",
  "url": "https://...",
  "selected": true
}
```

- `type`: `"journal"` | `"conference"` | `"workshop"`
- `selected: true` → appears on homepage

## Updating experience

Edit the `<ul class="exp-list">` block in `index.html`.
```

- [ ] **Step 3: Commit README and push to deploy**

```bash
git add README.md
git commit -m "docs: update README with site info and editing guide"
git push origin main
```

- [ ] **Step 4: Verify live site**

Wait ~60 seconds, then open `https://lukas-kirchdorfer.github.io` in browser.

Check:
- Site loads (not 404)
- Both pages render correctly
- Publications load from JSON (fetch works over HTTPS)

---

## Self-Review

**Spec coverage check:**
- Nav bar on both pages ✓ (Tasks 3, 4)
- Intro with name, subtitle, bio, social links ✓ (Task 3)
- Selected publications on homepage (JSON-driven, `selected: true`, year-sorted) ✓ (Task 3)
- Experience section hardcoded ✓ (Task 3)
- Publications subpage with Journals / Conferences / Workshops sections ✓ (Task 4)
- Year-descending sort within each section ✓ (Task 4, `populateSection`)
- Empty sections hidden ✓ (Task 4, `.hidden` class + conditional remove)
- Correct social link URLs ✓ (Task 3)
- Shared stylesheet ✓ (Task 1)
- JSON schema with all required fields ✓ (Task 2)
- README updated ✓ (Task 5)
- Deploy to GitHub Pages ✓ (Task 5)

**Placeholder scan:** None found.

**Type consistency:**
- `pub.selected`, `pub.type`, `pub.url`, `pub.title`, `pub.authors`, `pub.venue`, `pub.year` — consistent across Tasks 2, 3, 4 ✓
- CSS classes: `.pub-entry`, `.pub-title`, `.pub-meta`, `.pub-list`, `.exp-list`, `.exp-entry`, `.exp-role`, `.exp-date`, `.hidden` — defined in Task 1, consumed correctly in Tasks 3, 4 ✓
- Element IDs: `selected-pub-list` (Task 3), `journal-list`, `conference-list`, `workshop-list`, `journals`, `conferences`, `workshops` (Task 4) — no conflicts ✓

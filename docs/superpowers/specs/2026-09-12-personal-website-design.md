# Personal Website Design Spec
**Date:** 2026-09-12
**Owner:** Lukas Kirchdorfer
**Status:** Approved

---

## Overview

A minimal, clean personal academic website hosted on GitHub Pages at `lukas-kirchdorfer.github.io`. Modeled after the Jannik Brinkmann academic site style: text-first, monochromatic, no decorative elements. Two pages: a main landing page and an all-publications subpage.

---

## Goals

- Establish a professional online presence as a Senior AI Scientist / PhD
- Surface selected publications and professional experience on the homepage
- Provide a complete, well-organized publications list on a dedicated subpage
- Make it trivial to add new publications (edit one JSON file, no HTML changes)

---

## Architecture

**Approach:** Static HTML + CSS + JSON, no build step. GitHub Pages serves files directly.

```
lukas-kirchdorfer.github.io/
├── index.html              # Main page
├── publications.html       # All publications subpage
├── style.css               # Shared styles (single stylesheet)
└── data/
    └── publications.json   # Single source of truth for all papers
```

Both HTML files fetch `data/publications.json` at load time and render publication lists via vanilla JavaScript. No frameworks, no build tooling, no dependencies.

---

## Visual Style

- **Background:** White (`#fff`)
- **Text:** Near-black (`#111` / `#222`)
- **Links:** Muted (`#555` default, subtle on hover)
- **Layout:** Single centered column, `max-width: 700px`, generous padding (`2rem` sides)
- **Typography:** System font stack: `-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`
- **Section headers:** Slightly larger weight, small letter-spacing
- **No images, no decorative elements, no color accents**

---

## Pages

### `index.html` — Main Page

Top-to-bottom structure:

1. **Nav bar**
   - Left: `Lukas Kirchdorfer` (plain text or subtle link to `#top`)
   - Right: `Publications` link → `publications.html`

2. **Intro section**
   - `<h1>` name
   - Subtitle: "Senior AI Scientist at SAP · PhD in Artificial Intelligence"
   - 2–3 sentence bio
   - Icon/text links: [Google Scholar] [GitHub] [LinkedIn]

3. **Selected Publications section**
   - Heading: "Selected Publications"
   - Renders all entries from `publications.json` where `"selected": true`
   - Sorted by year descending
   - Each entry: title (hyperlinked to paper URL), authors, venue + year on one line
   - Link at bottom: "All publications →" → `publications.html`

4. **Experience section**
   - Heading: "Experience"
   - Hardcoded list, most-recent-first
   - Each entry: role + company, date range, optional one-line description

---

### `publications.html` — All Publications

Top-to-bottom structure:

1. **Same nav bar** — right side shows `Home` link instead of `Publications`

2. **Three sections in order:**
   - **Journals** — entries where `type === "journal"`, year descending
   - **Conferences** — entries where `type === "conference"`, year descending
   - **Workshops** — entries where `type === "workshop"`, year descending

3. **Each entry format** (same as homepage):
   - Title (hyperlinked to paper URL)
   - Authors
   - Venue + year

Sections with zero entries are hidden automatically.

---

## Data Schema

**`data/publications.json`:**

```json
{
  "publications": [
    {
      "title": "Paper Title Here",
      "authors": "L. Kirchdorfer, A. Coauthor, B. Coauthor",
      "venue": "NeurIPS",
      "year": 2024,
      "type": "conference",
      "url": "https://arxiv.org/abs/...",
      "selected": true
    }
  ]
}
```

**Field definitions:**

| Field | Type | Description |
|-------|------|-------------|
| `title` | string | Full paper title |
| `authors` | string | Author list, last initials first convention |
| `venue` | string | Journal/conference/workshop name |
| `year` | number | Publication year |
| `type` | enum | `"journal"` \| `"conference"` \| `"workshop"` |
| `url` | string | Link to paper (arXiv, ACL, DOI, etc.) |
| `selected` | boolean | `true` = appears on homepage selected list |

---

## External Links

| Link | Destination |
|------|-------------|
| Google Scholar | `https://scholar.google.de/citations?hl=de&user=4NuUVSkAAAAJ` |
| GitHub | `https://github.com/lukas-kirchdorfer` (to be confirmed) |
| LinkedIn | LinkedIn profile URL (to be confirmed) |

---

## Out of Scope

- Profile photo
- Dark mode
- Search/filter on publications page
- RSS feed
- Analytics
- Contact form
- Blog

---

## Implementation Notes

- Publications JSON is pre-populated with placeholder entries; owner replaces with real papers
- Experience section is hardcoded HTML (changes rarely)
- GitHub Pages deploys automatically on push to `main`
- No `.gitignore` entries needed (no secrets, no build artifacts)

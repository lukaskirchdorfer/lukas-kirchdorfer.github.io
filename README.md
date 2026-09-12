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

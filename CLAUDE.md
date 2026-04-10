# CLAUDE.md — Personal Page

This file describes the structure, conventions, and design principles of this site so that future work stays consistent.

---

## Deployment

- Hosted on **GitHub Pages** — whatever is on `main` is live at `https://cangdongcheng.github.io`
- No build step. All files are plain HTML/CSS/JS — edit and push directly.

---

## File Structure

```
/
├── index.html        # Home / hero + featured project cards
├── about.html        # Bio (with photo), education, experience
├── projects.html     # Project grid + inline detail modal
├── skills.html       # Interactive skills page
├── contact.html      # Contact information
├── data/
│   ├── projects.json # ← all project content lives here
│   └── skills.json   # ← all skill categories live here
├── content/          # Reference markdowns (sanitised summaries of source docs)
│   ├── fyp.md
│   ├── tll-internship.md
│   ├── kla-internship.md   # ⚠ KLA content is HIGH confidentiality
│   ├── imaging-course.md
│   └── bio.md
├── images/
│   └── profile.jpg   # Profile photo (circular, 600×600)
├── css/
│   └── style.css     # Single global stylesheet
├── .gitignore        # Blocks *.pdf, *.doc, *.docx, *.odt
├── CLAUDE.md
└── README.md
```

### Key architecture decisions
- **No separate project detail pages.** Featured projects open an inline modal on `projects.html`. This is the "last layer" — no further redirects.
- **All content is data-driven.** `projects.json` and `skills.json` are the only files to edit when updating content. HTML pages render dynamically via `fetch()`.
- **Homepage** links featured cards to `projects.html` where the modal provides full detail.

### Adding a new page
- Copy the nav block from any existing page.
- Add the new `<li>` to the nav in **all** existing HTML files.

---

## Updating Content

### Adding or editing a project
Edit `data/projects.json`. Each entry:
```json
{
  "id": "project-foo",
  "title": "Project Title",
  "description": "Short card description (1 sentence).",
  "skills": ["Python", "FEM", "Electrophysiology"],
  "tags": ["Python", "FEM"],
  "status": "In Progress",
  "featured": true,
  "details": {
    "overview": "Extended description for the detail modal.",
    "approach": "Methodology, tools, frameworks used.",
    "outcomes": "Results or current status.",
    "links": [
      { "label": "GitHub", "url": "https://github.com/..." }
    ]
  }
}
```

Field reference:
- `description` — short text shown on the card (non-featured projects only)
- `skills` — full list for filtering; must match names in `skills.json` exactly
- `tags` — display subset shown on the card (keep to 3–4)
- `status` — `"In Progress"` (amber), `"Class Project"` (blue), `"Internship"` (green), or omit for no badge
- `featured` — appears on homepage + projects page; card shows `Details →` button opening the modal
- `details` — content for the inline modal: `overview`, `approach`, `outcomes`, `links` (all optional; `overview` falls back to `description`)

### Adding a new skill
1. Add the name to the right category in `data/skills.json`.
2. Ensure at least one project in `projects.json` has that skill in its `skills` array.

### Removing a project
Delete its entry from `projects.json`. No other files need changing.

---

## Skills ↔ Projects Cross-Linking

- Each skill pill in `skills.html` links to `projects.html?skill=<name>` (rendered from `skills.json`).
- `projects.html` fetches `projects.json`, reads `?skill=`, and matches case-insensitively against each project's `skills` array.
- Matching cards get `.highlighted`; non-matching cards get `.dimmed`.
- A dismissible filter banner appears at the top.

**Rule:** skill names must be spelled identically in `skills.json` and in project `skills` arrays.

---

## Detail Modal (projects.html)

Featured project cards display a `Details →` button instead of a description paragraph. Clicking it opens an inline modal populated from the project's `details` object:
- **Overview** — extended description
- **Approach** — methodology and tools
- **Outcomes** — results or current status
- **Links** — external resources (shown only if non-empty)

Close via `×` button, clicking the backdrop, or pressing `Escape`.

---

## Local Development

`fetch()` calls won't work over `file://`. Run a local server from the repo root:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

---

## Design Principles

### Aesthetic
- **Terminal / academic monospace** — headings use `IBM Plex Mono`; body uses `Inter`.
- `>` logo prefix and `// comment` labels reinforce this tone.
- Restrained palette: `#fafbfc` background, `#0f4c75` accent, `#555e68` muted. Avoid new brand colors.

### CSS conventions
- All styles in `css/style.css`. No `<style>` blocks in pages.
- Use `:root` custom properties — do not hardcode colors.

### HTML / JS conventions
- No frameworks — zero dependency footprint.
- JS is inline at the bottom of each page that needs it.
- Project cards and skill pills are rendered from JSON — never hardcode content in HTML.

### Content tone
- Short, specific. No filler.
- Project descriptions: *what → how → outcome*.

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
├── index.html                    # Home / hero + featured projects
├── about.html                    # Bio, education, experience
├── projects.html                 # Project grid (filterable by skill)
├── skills.html                   # Interactive skills page
├── contact.html                  # Contact information
├── data/
│   ├── projects.json             # ← add/edit/remove projects here
│   └── skills.json               # ← add/edit/remove skills here
├── projects/
│   ├── project-ep-fem.html       # EP FEM simulation detail
│   ├── project-pino.html         # Neural operator detail
│   ├── project-biomed-imaging.html
│   ├── project-cpu.html
│   └── project-tll.html
├── css/
│   └── style.css                 # Single global stylesheet
└── CLAUDE.md
```

### Adding a new page
- Copy the nav block from any existing page.
- Add the new `<li>` to the nav in **all** existing pages.
- Link pages relative to root (e.g. `href="skills.html"`), except inside `projects/` where paths go up one level (`href="../skills.html"`).

---

## Updating Content

### Adding or editing a project
Edit `data/projects.json`. Each entry:
```json
{
  "id": "project-foo",
  "title": "Project Title",
  "description": "One or two sentences: problem, approach, outcome.",
  "skills": ["Python", "FEM", "Electrophysiology"],
  "tags": ["Python", "FEM"],
  "link": "projects/project-foo.html",
  "status": "In Progress",
  "featured": true
}
```
- `skills` — full list used for filtering; must match names in `skills.json` exactly (case-insensitive match at runtime)
- `tags` — display subset shown on the card (keep to 3–4)
- `status` — optional; `"In Progress"` (amber), `"Class Project"` (blue), `"Internship"` (green), or omit for no badge
- `featured: true` — project appears on the homepage; all featured projects are shown

Then create the detail page at `projects/project-foo.html` (copy any existing detail page as a template — note the `../` prefix on all asset paths).

### Adding a new skill
1. Add the name to the right category in `data/skills.json`.
2. Make sure at least one project in `projects.json` has that skill in its `skills` array — otherwise the pill links to an empty filter.

### Removing a project
Delete its entry from `projects.json`. No other files need changing.

---

## Skills ↔ Projects Cross-Linking

- Each skill pill in `skills.html` links to `projects.html?skill=<name>` (rendered from `skills.json`).
- On load, `projects.html` fetches `projects.json`, reads `?skill=`, and matches case-insensitively against each project's `skills` array.
- Matching cards get `.highlighted` (glowing border); non-matching cards get `.dimmed` (faded).
- A dismissible filter banner appears at the top.

**Rule:** skill names must be spelled identically in `skills.json` and in project `skills` arrays — the display name always comes from `skills.json`.

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
- **Terminal / academic monospace** feel — headings and UI elements use `IBM Plex Mono`; body copy uses `Inter`.
- The `>` logo prefix and `// comment` section labels reinforce this tone. Keep new sections consistent.
- Restrained color palette: near-white background (`#fafbfc`), dark-navy accent (`#0f4c75`), muted text (`#555e68`). Avoid introducing new brand colors.

### CSS conventions
- All styles live in `css/style.css`. Do not add `<style>` blocks to individual pages.
- Use the custom properties in `:root` (e.g. `var(--accent)`, `var(--border)`) — do not hardcode colors.
- Inline `style=""` is acceptable only for one-off layout nudges (e.g. `margin-top`), not for anything reusable.

### HTML / JS conventions
- No external CSS or JS frameworks — zero dependency footprint.
- JavaScript is used sparingly, inline at the bottom of the page that needs it.
- Project cards and skill pills are rendered entirely from JSON — do not hardcode content in HTML.

### Content tone
- Short, specific. Avoid filler phrases.
- Project descriptions: *what problem → what approach → what outcome*.

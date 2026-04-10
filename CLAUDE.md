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
├── index.html          # Home / hero page
├── about.html          # Bio, education, skills summary
├── projects.html       # Project grid (filterable by skill)
├── skills.html         # Interactive skills page — links into projects
├── contact.html        # Contact information
├── projects/
│   └── project-1.html  # Individual project detail page (template)
├── css/
│   └── style.css       # Single global stylesheet — all styles live here
└── CLAUDE.md           # This file
```

### Adding a new page
- Copy the nav block from any existing page.
- Add the new `<li>` to the nav in **all** existing pages.
- Link pages relative to root (e.g. `href="skills.html"`), except inside `projects/` where paths go up one level (`href="../skills.html"`).

### Adding a new project
1. Add a card to `projects.html` with a `data-skills="Skill1,Skill2,..."` attribute. Skill names must exactly match the labels used in `skills.html`.
2. Create a detail page under `projects/project-N.html` (copy `project-1.html` as a template — note the `../` prefix on all asset paths).

---

## Skills ↔ Projects Cross-Linking

Skills and projects are linked via URL query parameters:

- Each skill pill in `skills.html` links to `projects.html?skill=<name>`.
- On load, `projects.html` reads `?skill=` and compares it (case-insensitive) against each card's `data-skills` comma-separated list.
- Matching cards get `.highlighted` (glowing border), non-matching cards get `.dimmed` (faded).
- A dismissible filter banner is shown at the top.

**Rule:** whenever a new skill is added to `skills.html`, ensure at least one project card in `projects.html` carries it in `data-skills`, and vice versa — orphan skills or untagged cards break the cross-link.

---

## Design Principles

### Aesthetic
- **Terminal / academic monospace** feel — headings and UI elements use `IBM Plex Mono`; body copy uses `Inter`.
- The `>` logo prefix and `// comment` section labels reinforce this tone. Keep new sections consistent with this pattern.
- Restrained color palette: near-white background (`#fafbfc`), dark-navy accent (`#0f4c75`), muted text (`#555e68`). Avoid introducing new brand colors.

### CSS conventions
- All styles live in `css/style.css`. Do not add `<style>` blocks to individual pages.
- Use the custom properties defined in `:root` (e.g. `var(--accent)`, `var(--border)`) — do not hardcode colors.
- Inline `style=""` attributes are acceptable only for one-off layout nudges (e.g. `margin-top`), not for anything reusable.

### HTML conventions
- No external CSS or JS frameworks — keep the dependency footprint at zero.
- JavaScript is used sparingly and only inline at the bottom of the page that needs it. Keep it small and self-contained.
- Tag content with `data-skills` on project cards; never encode filtering logic into class names.

### Content tone
- Short, specific, first-person. Avoid filler phrases ("feel free to", "don't hesitate to").
- Project descriptions follow the pattern: *what problem → what approach → what outcome*.

---

## Updating Content (for real projects)

When replacing placeholder content with real projects:
1. Update the card in `projects.html` (title, description, tags, `data-skills`, link).
2. Fill in the corresponding `projects/project-N.html` detail page (Overview, Approach, Results, Links).
3. Check that all skill names in `data-skills` exist in `skills.html` — add new pills there if needed.
4. Optionally update the "Featured Work" cards on `index.html` to reflect the top 3 projects.

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
├── projects.html     # Blog-style feed of featured projects + compact cards for others
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
├── docs/
│   └── resume.pdf    # Resume/CV download — linked from homepage hero. LaTeX source lives at ~/Projects/Resume/Cang_Dongcheng_CV.tex (separate repo); rebuild with `tectonic` and copy the PDF here.
├── css/
│   └── style.css     # Single global stylesheet
├── .gitignore        # Blocks *.pdf, *.doc, *.docx, *.odt
├── CLAUDE.md
└── README.md
```

### Key architecture decisions
- **No separate project detail pages.** Featured projects render as full inline blog posts on `projects.html` — all content visible, no modals, no redirects.
- **All content is data-driven.** `projects.json` and `skills.json` are the only files to edit when updating content. HTML pages render dynamically via `fetch()`.
- **Homepage** links featured cards to `projects.html` where the blog feed lives.
- **Scope is broad.** Featured Work is not limited to computational health / BME — community work (e.g. Cultural Kaleidoscope) is welcome.

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
    "overview": "Extended description for the blog post.",
    "approach": "Methodology, tools, frameworks used.",
    "outcomes": "Results or current status.",
    "links": [
      { "label": "GitHub", "url": "https://github.com/..." }
    ],
    "media": {
      "type": "video",
      "src": "images/my-simulation.mp4",
      "caption": "Description of the media"
    }
  }
}
```

Field reference:
- `description` — short text shown on the card (non-featured projects only)
- `skills` — full list for filtering; must match names in `skills.json` exactly
- `tags` — display subset shown on the card/header (keep to 3–4)
- `status` — optional badge:
  - `"In Progress"` — amber
  - `"Class Project"` — blue
  - `"Internship"` — green
  - `"Community"` — purple
- `featured` — if true, renders as a full blog post on `projects.html` and appears on the homepage. If false, renders as a compact card under "Other Projects".
- `details` — content for the blog post (featured only). All fields optional; `overview` falls back to `description` if missing.
- `details.media` — optional. Types:
  - `"placeholder"` — dashed box with a caption (for "coming soon")
  - `"video"` — HTML5 video with `src` and optional `caption`
  - `"image"` — inline image with `src` and optional `caption`

### Adding a new skill
1. Add the name to the right category in `data/skills.json`.
2. Ensure at least one project in `projects.json` has that skill in its `skills` array.

### Removing a project
Delete its entry from `projects.json`. No other files need changing.

---

## Skills ↔ Projects Cross-Linking

- Each skill pill in `skills.html` links to `projects.html?skill=<name>` (rendered from `skills.json`).
- `projects.html` fetches `projects.json`, reads `?skill=`, and matches case-insensitively against each project's `skills` array.
- Matching blog posts / cards get `.highlighted` (glowing border); non-matching get `.dimmed` (faded).
- A dismissible filter banner appears at the top.

**Rule:** skill names must be spelled identically in `skills.json` and in project `skills` arrays.

---

## Blog Feed (projects.html)

Featured projects render as full-width `<article class="blog-post">` elements, each showing:
- Status badge, title, tags
- Optional media section (video / image / placeholder)
- **Overview** — extended description
- **Approach** — methodology and tools
- **Outcomes** — results or current status
- **Links** — external resources (shown only if non-empty)

Non-featured projects render as compact cards in a separate "Other Projects" grid below the feed.

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
- Restrained palette: `#fafbfc` background, `#0f4c75` accent, `#555e68` muted. Status badges use muted pastels (amber / blue / green / purple). Avoid new brand colors.

### CSS conventions
- All styles in `css/style.css`. No `<style>` blocks in pages.
- Use `:root` custom properties — do not hardcode colors.

### HTML / JS conventions
- No frameworks — zero dependency footprint.
- JS is inline at the bottom of each page that needs it.
- Project cards and skill pills are rendered from JSON — never hardcode content in HTML.

### Content tone
- Short, specific. No filler, no cliches.
- Project descriptions: *what → how → outcome*.
- **Confidentiality:** KLA content must stay general (cleanroom, microscopy, modular software dev). No laser specs, contamination details, or proprietary methods. See `content/kla-internship.md` for the safe-content rule.

---

## Acknowledgements

This site was built in collaboration with Claude (Anthropic's Sonnet 4.6). The footer across all pages reads "Built with care, thanks to Claude" as credit.

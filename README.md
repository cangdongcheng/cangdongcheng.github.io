# cangdongcheng.github.io

Personal academic portfolio for Dongcheng Cang — hosted at **https://cangdongcheng.github.io**

## About

Senior undergraduate in Biomedical Engineering at the National University of Singapore (NUS). This site showcases research projects, technical skills, and experience in computational modelling, biomedical imaging, and wet lab work.

## Tech Stack

- Plain HTML, CSS, and vanilla JS — no frameworks, no build step
- Fonts: IBM Plex Mono (headings) + Inter (body) via Google Fonts
- Hosted on GitHub Pages — `main` branch deploys automatically

## Content is Data-Driven

Projects and skills are stored as JSON and rendered dynamically:

| File | Purpose |
|------|---------|
| `data/projects.json` | Add, edit, or remove projects here |
| `data/skills.json` | Add, edit, or remove skills here |

Skills on the Skills page link directly to filtered project views via URL query params (`?skill=Python`).

## Local Development

`fetch()` calls won't work over `file://`. Serve locally with:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Pages

| Page | Description |
|------|-------------|
| `index.html` | Home / featured projects |
| `about.html` | Bio, education, experience |
| `projects.html` | Full project grid with skill filtering |
| `skills.html` | Interactive skills — click to filter projects |
| `contact.html` | Contact information |
| `projects/*.html` | Individual project detail pages |

# HANDOFF.md — Session Continuity

A living document for carrying context across Claude sessions. Read this first when picking up work on the personal page.

> **For architecture and conventions, see `CLAUDE.md`.**
> This file is specifically about *state*: what's done, what's pending, and what has been learned about the user's preferences.

---

## Owner

**Dongcheng Cang** — senior B.Eng. Biomedical Engineering at NUS, doing a Final Year Project under Dr Lei Li (Digital Heart Lab) on *Physics-informed ECG surrogate model for efficient cardiac digital twin*. Interested in computational cardiology, physics-informed ML, biomedical imaging, and AI agents.

---

## Current State Snapshot

### Site contents (as of latest session)

**Featured projects** (5) — rendered as full blog posts on `projects.html`:
1. **Cardiac EP Simulation with openCARP (Final Year Project)** — Track 1 of FYP
2. **Physics-Informed Neural Surrogate for Cardiac Digital Twin (Final Year Project)** — Track 2 of FYP (DIMON + PINNs extensions)
3. **Biomedical Imaging with AI (BN3406)** — U-Net, VoxelMorph, CMRx4DFlow MICCAI 2026 Challenge
4. **CPU Design from NAND Gates** — class project, HDL
5. **Cultural Kaleidoscope @ UTown Residence** — community event, 2024 + 2025 editions, 300+ residents
6. **TLL Protein Characterization (Arabidopsis RRM1)** — wet lab internship

**Other projects** (non-featured, compact cards):
- KLA Singapore R&D Intern
- ECG Acquisition & Biosignal Processing (class)

### Pages
- `index.html` — hero + featured cards + Resume button (link is a placeholder)
- `about.html` — bio (with circular photo), education, experience, hidden "What Drives Me" and "Others" sections (commented out, can re-enable)
- `projects.html` — blog feed + compact cards for others
- `skills.html` — interactive pills
- `contact.html` — email set to `e1032484@u.nus.edu`

---

## Known Placeholders & TODOs

| Item | Location | What's needed |
|------|----------|---------------|
| openCARP animation | `data/projects.json` → `project-ep-fem.details.media` | Placeholder in place (`type: "placeholder"`). Add a video/gif file, change `type` to `"video"` or `"image"`, add `src` |
| LinkedIn URL | `contact.html` | Still `linkedin.com/in/your-profile` |
| Google Scholar | `contact.html` | Placeholder link — remove if not applicable |
| "Previous Education" entry | `about.html` | Currently Hwa Chong Institution 2020–2021 — minimal, could add more context |
| Publications & Awards | `about.html` | Section exists, currently empty ("muted" note) |
| 2 more projects | User mentioned wanting to add 2 more projects beyond the current set |

---

## User Preferences & Feedback (learned over time)

### Content tone
- **Short, specific, no cliches.** User explicitly rejected "What Drives Me" and "Others" sections as cringe and asked to comment them out. Keep descriptions factual.
- **Avoid "Track 1 / Track 2"** in FYP project titles — use "(Final Year Project)" instead.
- **"Physiology"**, not "biology", in the hero description.
- **Featured Work scope is broad** — not just computational health / BME. Community work (Cultural Kaleidoscope) is welcome.

### Technical preferences
- User prefers **inline content** over modals or redirects. The projects page went from cards → modal → blog feed based on this preference.
- User explicitly does not want to emphasize **PyTorch** ("I vibe code all the time") — kept out of skills page though it may appear in project details.
- User removed **LaTeX** from skills ("not very good at it").
- User does NOT want to mention **collaboration** — changed "research collaborations" to "sharing research interests" on contact page.

### Design
- Profile photo was tried on homepage hero but removed — "doesn't look good". Photo only lives on About page now (circular, faded edges, 180px, `object-position: center 35%` to cut sky).
- User wanted the photo centered on them since they weren't centered in the original image — cropped via Pillow script.
- Footer across all pages reads `"Built with care, thanks to Claude"` (user requested self-reference).

### Confidentiality
- **KLA content is HIGH confidentiality.** Only mention cleanroom, microscopy (DIC, dark-field), modular software dev, semiconductor industry. NO laser specs, contamination details, proprietary tools, team names, or mentor names.
- TLL content is low-confidentiality (academic research) — Arabidopsis / RRM1 / PCR / SEM all fair game.

---

## Key Architecture Decisions

See `CLAUDE.md` for full detail, but in short:
1. **Data-driven.** All content in `data/projects.json` and `data/skills.json`. HTML renders via `fetch()`.
2. **No separate project detail pages.** Featured projects are full-width blog posts inline on `projects.html`.
3. **No build step.** Plain HTML/CSS/JS, deployed to GitHub Pages from `main`.
4. **Single CSS file.** `css/style.css`. Use `:root` custom properties.
5. **Skills ↔ projects cross-linking** via URL query params (`?skill=Python`).

---

## Resume / CV

The CV linked from the homepage (`docs/resume.pdf`) is built from a LaTeX source kept in a **separate** project at `~/Projects/Resume/Cang_Dongcheng_CV.tex`. Built with **Tectonic** (`tectonic Cang_Dongcheng_CV.tex` from that dir). When the source changes, copy `Cang_Dongcheng_CV.pdf` over `docs/resume.pdf` and commit.

Format: 2-page single-column custom minimalist style, research-CV oriented (PhD-applicant flavour). Latin Modern Roman default fonts, no fontspec.

---

## Source Materials (outside repo, gitignored)

Located at `/home/cdc/Projects/Personal_page/personal_page material/`:
- `Cang Dongcheng - TLL internship report.pdf`
- `Final Report Cang Dongcheng.pdf` (KLA — confidential)
- `FYP Proposal Form.doc`
- `Preliminary Report 2 - Cang Dongcheng - A0264672X.pdf`
- `BN3406 - Lecture 1.pdf` through `Lecture 5.pdf`
- `Weixin Image_*.jpg` (profile photo source)

`.gitignore` blocks `*.pdf`, `*.doc`, `*.docx`, `*.odt` so these can't accidentally be committed.

Sanitised summaries of these documents live in `content/*.md` inside the repo.

---

## Dev Workflow

```bash
# Serve locally (fetch() requires HTTP, not file://)
python3 -m http.server 8000

# Then open http://localhost:8000
```

After editing:
```bash
git add <files>
git commit -m "..."
git push
```

GitHub Pages auto-deploys from `main` — usually live within 1–2 minutes.

---

## When Starting a New Session

1. Read `CLAUDE.md` for architecture
2. Read this file (`HANDOFF.md`) for state and preferences
3. Check `git log --oneline -10` for recent work
4. Ask the user what they want to work on — don't assume from context
5. Respect the confidentiality rules around KLA content

---

## Update Rhythm

**Update this file whenever:**
- A placeholder is filled (remove from the TODO table)
- A new preference is learned (add to the preferences section)
- A significant architectural change happens (but most structural changes go in `CLAUDE.md`)
- New source materials are added or confidentiality rules change

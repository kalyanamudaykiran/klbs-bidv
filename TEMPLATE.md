# BIDV Course Site — Build Spec

**Locked:** 2026-11-18 · **Course:** 26MB5210E · **Dept of MBA (KLVZA)** · **KLU KL Business School**

Single source of truth for every new module (CO2 → CO6). Build against this, not against memory.

---

## 1 · Course identity (from the syllabus PDF — authoritative)

- **Course code: 26MB5210E.** The PDF's *filename* says 26MB5102E — that is the **prerequisite**
  course code (Statistics for Business Decisions); do not use it.
- Even Sem 2026-27, L-T-P-S 2-1-3-0, 3 credits.
- Coordinator: Hema Divya Kantamaneni · Instructors: Hema Divya Kantamaneni, Venkateswara Kumar K S,
  Uday Kalyanam (copy into every footer, verbatim from the PDF page 1).
- Tools: **Power BI and Tableau**. No R, no Python, no Colab anywhere in this course.

## 2 · Module → CO map (six modules, one per CO — from the syllabus)

| CO | Module | State |
|---|---|---|
| CO1 | Analytics Taxonomy & the Data Lifecycle | **shipped** (`co1-theory.html` + `co1-labs.html`) |
| CO2 | KPI & Metric Design | coming soon |
| CO3 | Visualization Principles & Chart Selection | coming soon |
| CO4 | Dashboard Build in Power BI / Tableau | coming soon |
| CO5 | Data Storytelling | coming soon |
| CO6 | BI Architecture & Self-Service Analytics | coming soon |

## 3 · How this course differs from PFB (the locked deviations)

PFB's pattern is a 1280×720 slide deck + a Colab instructions page + an `.ipynb`. **None of that
applies here.** BIDV's courseware is *interactive, self-contained web apps*:

| PFB | BIDV |
|---|---|
| `co{X}-theory.html` = 1280×720 `.stage` slide deck, hash routing, keyboard nav | `co{X}-theory.html` = tabbed **theory-and-simulation dashboard** (quizzes + note boxes + a decision sim + a Submission tab) |
| `co{X}-labs.html` = instructions page + "Open in Colab" button | `co{X}-labs.html` = the **labs themselves**, running in the browser (seeded-RNG simulations, live SVG charts, optimizer) |
| `co{X}-lab.ipynb` uploaded to Drive | **no notebooks, no Drive** — everything is one self-contained HTML file per page |
| no student submission mechanism | **both pages capture submissions**: name + student ID, `localStorage`, a Submission tab, `@media print` PDF + msword-Blob `.doc` download, files named `BI_Module{X}_{ID}` / `BIDV_Labs{X}_{ID}` |

Consequences:
- Every page must stay **fully self-contained** (no CDN, no external JS/CSS/images, no fetch).
  Students open one file and it works offline.
- `localStorage` keys are **namespaced per course + page**: `bidv.co1` (theory), `bidv.co1l` (labs).
  The PFB and BIDV sites share the `github.io` origin — never reuse a key across courses.

## 4 · Shared header (every page, both styles)

A 28-px-ish navy bar (`#0F172A`) at the very top: brand `BIDV · 26MB5210E` (amber `#F5B041`,
letter-spaced) · crumb `Course / {page}` (Course links to `index.html`) · grow spacer ·
reciprocal button (theory ⇄ labs, amber `btn` or ghost variant) · `All modules` ghost button.
Mirror of PFB's 4-button header rule, adapted: theory and labs pages **do** carry the reciprocal
button because the two pages are a single navigable unit here (unlike PFB, where slides and
Colab never meet). The hub `index.html` carries no header — it *is* the nav.
Print: `.top` is hidden under `@media print`.

## 5 · Hub `index.html`

PFB's grid verbatim (navy `#0F172A`, amber `#F5B041`): 1–2–2–2 —
row 1 = full-width "START HERE" tile (blue-glass intro card, single `Start CO1` amber button);
rows 2–4 = CO1–CO6 tiles, each `CO{n} · {KICK}` + module title; shipped = two buttons
(`Theory` + `Labs`), unshipped = `card.soon` + "Coming soon". Faculty + coordinator footer
(§1). Sub-line carries the L-T-P-S structure (`2-1-3-0`).

### Tile promotion (when a module ships)

1. `class="card soon"` → `class="card"`
2. Replace `<span class="tag-soon">Coming soon</span>` with the two buttons
3. No other edits — the `START HERE` tile keeps pointing at CO1.

## 6 · File naming

| Purpose | File |
|---|---|
| Hub | `index.html` |
| Theory + simulation dashboard | `co{X}-theory.html` |
| In-browser labs | `co{X}-labs.html` |
| Build spec (this file) | `TEMPLATE.md` |

## 7 · The CO1 page pattern (copy for CO2+)

**Theory dashboard:** JS-built tabs (Theory card | Tutorial card), one-shot-lock quizzes with
reveal + per-question "What did I learn" note boxes, one decision simulation with live KPIs,
a **write-up prompt rendered into the Tutorial card and the report** (`pr` field — never
defined-but-unused), a Submission tab that gates on name+ID and short notes, then PDF (print)
or `.doc` (msword Blob). All state in one `localStorage` key; restore on load.

**Labs page:** static sections behind a tab bar, seeded RNG (`mulberry32` + Box-Muller) so
"new sample" is reproducible, live SVG (no chart libs), optimizer where relevant. Must include:
name+ID inputs, **writing captures for every prompt** (no uncaptured instructions), a
Submission section with print + `.doc`, `localStorage` persistence (score, writes, picks,
settings), and print CSS.

**KPIs are computed from the data arrays, never hard-coded** (the CO1 bakery KPIs were baked
in and wrong: weekend average is 42.5, not 43).

## 8 · Verification checklist (run before marking a module done)

- [ ] No placeholder course code anywhere (search for `XX`, `22MB62`, `26MB5102E`)
- [ ] Every `href`/`onclick` target exists; both module pages link to each other and to `index.html`
- [ ] Each writing prompt has a capture (textarea/note box) and appears in the Submission report
- [ ] No undefined state fields (no `I={}`-style vestiges)
- [ ] Every KPI computed from its data source
- [ ] `localStorage` keys namespaced `bidv.co{n}` / `bidv.co{n}l`; refresh restores all state
- [ ] Submission gated on name+ID; filenames `{CourseCode}_{ID}`
- [ ] `@media print` hides nav/header/inputs, keeps report + answers
- [ ] Pages fully self-contained (zero external requests) — verify with a network-blocked load
- [ ] Headless-browser smoke test at 2560×1440: every tab, every sim, every button
- [ ] Footer faculty list matches the syllabus PDF page 1

## 9 · Publishing

Public repo `kalyanamudaykiran/klbs-bidv` → GitHub Pages (`main`, `/` →
`https://kalyanamudaykiran.github.io/klbs-bidv/`, trailing slash mandatory). Every
`git push` to `main` auto-redeploys — no dashboard step.

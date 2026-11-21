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
| CO1 | Analytics Taxonomy & the Data Lifecycle | **shipped** (`co1-theory.html` slides + `co1-dashboard.html` + `co1-labs.html`) |
| CO2 | KPI & Metric Design | coming soon |
| CO3 | Visualization Principles & Chart Selection | coming soon |
| CO4 | Dashboard Build in Power BI / Tableau | coming soon |
| CO5 | Data Storytelling | coming soon |
| CO6 | BI Architecture & Self-Service Analytics | coming soon |

## 3 · How this course differs from PFB (the locked deviations)

PFB's pattern is a 1280×720 slide deck + a Colab instructions page + an `.ipynb`. BIDV mirrors
the **slide deck** but *drops the notebook* — its courseware adds interactive, self-contained
web apps:

| PFB | BIDV |
|---|---|
| `co{X}-theory.html` = 1280×720 `.stage` slide deck, hash routing, keyboard nav | **same, verbatim** — `co1-theory.html` is the deck (PFB CSS/nav/slide-number rules, `#14213D`/`#FAF7F0`/`#F5B041` tokens, 1280×720 stage, `#/n` hash, `fit()` + click/swipe/keys) |
| `co{X}-labs.html` = instructions page + "Open in Colab" button | `co{X}-labs.html` = the **labs themselves**, running in the browser (seeded-RNG simulations, live SVG charts, optimizer) |
| `co{X}-lab.ipynb` uploaded to Drive | **no notebooks, no Drive** — everything is one self-contained HTML file per page |
| no student submission mechanism | **both interactive pages capture submissions**: name + student ID, `localStorage`, a Submission tab, `@media print` PDF + msword-Blob `.doc` download, files named `BI_Module{X}_{ID}` / `BIDV_Labs{X}_{ID}` |
| — | **extra page PFB has not got**: `co{X}-dashboard.html` — the tabbed theory-and-simulation dashboard (quizzes + note boxes + a decision sim + write-up prompts + Submission tab). Shipped for CO1 only; add for later modules when the material calls for it |
| — | **`co{X}-notes.html`** — the exam-notes page (PFB CO1 pattern, shipped for BIDV CO1 on 2026-11-19): exam-framed theory notes per COI, one-table cheat sheets, a final checklist, and a fold-out practice paper (six 2-mark + six 5-mark, model answers). Shipped for CO1 only; add for later modules after the deck lands |

Consequences:
- Every page must stay **fully self-contained** (no CDN, no external JS/CSS/images, no fetch).
  Students open one file and it works offline.
- `localStorage` keys are **namespaced per course + page**: `bidv.co1` (dashboard), `bidv.co1l`
  (labs). The PFB and BIDV sites share the `github.io` origin — never reuse a key across courses.
- The slide deck carries **no interactivity** — it is presentation-only (PFB rule), so it holds no
  `localStorage` and no submission; the dashboard and labs are the only pages that capture work.

## 4 · Shared header (every page, both styles)

A 28-px-ish navy bar (`#0F172A`) at the very top: brand `BIDV · 26MB5210E` (amber `#F5B041`,
letter-spaced) · crumb `Course / {page}` (Course links to `index.html`) · grow spacer ·
reciprocal buttons (Slides ⇄ Notes ⇄ Practice ⇄ Labs, amber `btn` or ghost variant; the
`co1-dashboard.html` page is labelled **Practice**, not Dashboard) · `All modules`
ghost button. Each interactive page (practice, labs, notes) carries buttons for its siblings
*and* the deck; the deck's PFB-style header carries `Notes` + `Practice` + `Labs` +
`All modules`.
**Labs re-enabled 2026-11-21** (had been disabled 2026-11-19, commit 1aa071c): the `Labs`
buttons are back on the hub CO1 tile and the deck/practice/notes headers, and
`co1-labs.html` no longer carries a "switched off" banner. The current Canva-based deck
carries no in-slide chips, so nothing there needed re-targeting. Disabling again = drop
those four buttons + re-add the banner on `co1-labs.html`.
The hub `index.html` carries no header — it *is* the nav.
Print: `.top` is hidden under `@media print` on the interactive pages.

## 5 · Hub `index.html`

Navy `#0F172A` / amber `#F5B041` grid: row 1 = CO1 tile, rows 2–4 = CO2–CO6 (the former
full-width "START HERE" intro tile was removed 2026-11-19). Shipped tile = `Slides` +
`Notes` + `Practice` + `Labs` buttons (drop `Practice` where a module ships no
practice page, `Labs` where it ships no labs page), unshipped = `card.soon` + "Coming soon".
Faculty + coordinator footer
(§1). Sub-line carries the L-T-P-S structure (`2-1-3-0`).

### Tile promotion (when a module ships)

1. `class="card soon"` → `class="card"`
2. Replace `<span class="tag-soon">Coming soon</span>` with the buttons above
3. No other edits.

## 6 · File naming

| Purpose | File |
|---|---|
| Hub | `index.html` |
| Slide deck (PFB-style, 1280×720 stage) | `co{X}-theory.html` |
| Theory + simulation dashboard (interactive; CO1 only so far) | `co{X}-dashboard.html` |
| Exam notes: theory + cheat sheets + fold-out practice paper (CO1 only so far) | `co{X}-notes.html` |
| In-browser labs | `co{X}-labs.html` |
| Build spec (this file) | `TEMPLATE.md` |

## 7 · The CO1 page pattern (copy for CO2+)

**Slide deck (`co{X}-theory.html`, PFB-identical mechanics):** 1280×720 `.stage`, `fit()` scaling,
`#/n` hash routing, arrow/PageUp-Down/space keys, `F` fullscreen, tap/swipe. Slide kinds and
numbering (PFB rule, enforced): title · roadmap · COI dividers (objectives list,
"By the end of this lecture, students can:") · content slides · ACTIVITY slides (amber
`#F5B041` background, dark text, stretch-goal card) · recap slides (2×2 `#1F2F55` cards) ·
checklist. Numbered slides: every content/activity/recap/checklist slide gets a slide number
top-right (`#4A5568` on light, `#8FA3C0` on navy) *except* the title and dividers. Light slides
`#FAF7F0` (headings `#14213D`, kickers `#C2570C`), navy `#14213D` (headings `#FFFFFF`,
kickers `#F5B041`). Deck is presentation-only — no state, no `localStorage`.

**Theory dashboard (`co{X}-dashboard.html`):** JS-built tabs (Theory card | Tutorial card),
one-shot-lock quizzes with reveal + per-question "What did I learn" note boxes, one decision
simulation with live KPIs, a **write-up prompt rendered into the Tutorial card and the report**
(`pr` field — never defined-but-unused), a Submission tab that gates on name+ID and short
notes, then PDF (print) or `.doc` (msword Blob). All state in one `localStorage` key; restore
on load. Ship this page only when the module's material calls for interactive practice.

**Labs page:** static sections behind a tab bar, seeded RNG (`mulberry32` + Box-Muller) so
"new sample" is reproducible, live SVG (no chart libs), optimizer where relevant. Must include:
name+ID inputs, **writing captures for every prompt** (no uncaptured instructions), a
Submission section with print + `.doc`, `localStorage` persistence (score, writes, picks,
settings), and print CSS.

**KPIs are computed from the data arrays, never hard-coded** (the CO1 bakery KPIs were baked
in and wrong: weekend average is 42.5, not 43).

## 8 · Verification checklist (run before marking a module done)

- [ ] No placeholder course code anywhere (search for `XX`, `22MB62`, `26MB5102E`)
- [ ] Every `href`/`onclick` target exists; slides, dashboard, labs and hub link to each other
- [ ] **Slides:** `node --check` on the extracted nav script; slide count = section count;
      numbered slides match the PFB rule (numbers on content/activity/recap/checklist only);
      hash routing `#/n` + `fit()` present; no `localStorage`, no external requests
- [ ] Each writing prompt has a capture (textarea/note box) and appears in the Submission report
- [ ] No undefined state fields (no `I={}`-style vestiges)
- [ ] Every KPI computed from its data source
- [ ] `localStorage` keys namespaced `bidv.co{n}` (dashboard) / `bidv.co{n}l` (labs); refresh restores all state
- [ ] Submission gated on name+ID; filenames `{CourseCode}_{ID}`
- [ ] `@media print` hides nav/header/inputs, keeps report + answers
- [ ] Pages fully self-contained (zero external requests) — verify with a network-blocked load
- [ ] Headless-browser smoke test at 2560×1440: every tab, every sim, every button,
      and every slide (walk `#/1`..`#/n`)
- [ ] Footer faculty list matches the syllabus PDF page 1

## 9 · Publishing

Public repo `kalyanamudaykiran/klbs-bidv` → GitHub Pages (`main`, `/` →
`https://kalyanamudaykiran.github.io/klbs-bidv/`, trailing slash mandatory). Every
`git push` to `main` auto-redeploys — no dashboard step.

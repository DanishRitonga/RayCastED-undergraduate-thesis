# AGENTS.md

Bachelor thesis (LaTeX) for DTETI UGM — "RayCastED: A RayCast End-to-end Detection Model for Lightweight and Boundary-Aware Nuclei Detection in Histopathology Images".

## Build

```bash
latexmk -xelatex main.tex          # compile thesis (recommended)
latexmk -c                          # clean artifacts

# Slides (Beamer, inside slides/)
latexmk -xelatex slides/presentation.tex
```

Manual sequence without `latexmk`: `xelatex main.tex && bibtex main && xelatex main.tex && xelatex main.tex`

Missing `.sty` at compile time? Copy from `packages/` to project root.

## Key files

- `main.tex` — master document: metadata + `\include{}` of all content
- `thesisdtetiugm.cls` — custom `report`-based class (layout, cover, formatting). Options: `bachelor|master|doctoral` × `bahasa|english`. Current: `bachelor,english`
- `references.bib` — BibTeX (IEEEtran style)
- `slides/presentation.tex` — Beamer defense slides

## Content layout

All thesis content is under `contents/`:
- `abstract/` — `abstract.tex` (English) + `intisari.tex` (Indonesian) — both required
- `chapter-1/` through `chapter-6/` — main chapters (figures go in the same directory)
- `appendix/` — appendix sections
- Front matter: `preface/`, `dedication/`, `statement/`, `nomenclature/`

Document order: cover → endorsement → statement → dedication → preface → ToC/LoT/LoF → nomenclature → intisari → abstract → chapters 1–6 → references → appendices

## Conventions

- Custom commands from `thesisdtetiugm.cls` — do **not** redefine: `\printcover`, `\printendorsement`, `\chapterstatement`, `\chapterdedication`, `\chapterpreface`, `\chapternomenclature`, `\chapterintisari`, `\chapterabstract`, `\chapterappendix`, `\chapterappendixadd`, `\addsupervisor`, `\addexaminer`, `\startmain`
- Appendices use `L-` page numbering via `\chapterappendix` (first) and `\chapterappendixadd` (subsequent)
- `\include{}` (not `\input{}`) is used for chapters — each must start with `\cleardoublepage \phantomsection`
- `sample/` holds template assets (UGM logo, scanned docs) — not thesis content
- `main.tex` still contains placeholder values (supervisor names, exam date, etc.) — do not fill in real NIP/personal data
- Do not use em dashes (`---`). Use commas, parentheses, colons, or en dashes (`--`) instead.

## Companion codebase

Model implementation: `/home/danishrtg/projects/RayCastED/` — spec at `docs/project.md` in that repo.

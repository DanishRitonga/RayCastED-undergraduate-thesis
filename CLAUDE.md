# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Bachelor thesis for the Department of Electrical Engineering and Information Technology (DTETI), Universitas Gadjah Mada. The thesis is titled "RayCastED: A RayCast End-to-end Detection Model for Lightweight and Boundary-Aware Nuclei Detection in Histopathology Images" and is written in Biomedical Engineering.

**Companion codebase:** `/home/danishrtg/projects/RayCastED/` — the RayCastED raycast polygon detector built on YOLOv26 for cell detection in histopathological WSIs. Its authoritative specification is `docs/project.md` in that repo.

## Build Commands

```bash
# Compile the full thesis (requires latexmk + XeLaTeX/LuaLaTeX)
latexmk -xelatex main.tex

# Clean build artifacts
latexmk -c

# Compile without latexmk (manual sequence)
xelatex main.tex
bibtex main
xelatex main.tex
xelatex main.tex
```

Missing `.sty` files can be copied from the `packages/` directory into the project root.

## Architecture

This is a LaTeX thesis using a custom class file `thesisdtetiugm.cls` that handles all formatting for DTETI UGM theses.

**Key files:**
- `main.tex` — Master document: declares metadata (author, title, supervisors, examiners), document class options, and includes all content via `\include{}`
- `thesisdtetiugm.cls` — Custom class controlling layout, cover pages, and formatting. Supports options: `bachelor|master|doctoral` and `bahasa|english`
- `references.bib` — BibTeX bibliography (IEEEtran style)

**Content structure** (all under `contents/`):
- `abstract/` — `abstract.tex` (English) and `intisari.tex` (Indonesian)
- `chapter-1/` through `chapter-6/` — Main thesis chapters
- `appendix/` — Appendix sections
- `preface/`, `dedication/`, `statement/`, `nomenclature/` — Front matter pages

**Document order in main.tex:** cover → endorsement → statement → dedication → preface → ToC/LoT/LoF → nomenclature → Indonesian abstract → English abstract → chapters 1–6 → bibliography → appendices.

## Conventions

- Language is set to `english` in the document class options. Dual-language abstracts are required (English + Indonesian/Bahasa).
- Figures belonging to a chapter should be placed in that chapter's directory.
- The `sample/` directory contains only template placeholders (UGM logo, scanned documents, sample code) — not actual thesis content.
- The class file (`thesisdtetiugm.cls`) defines custom commands like `\printcover`, `\chapterstatement`, `\chapterdedication`, `\chapterpreface`, `\chapternomenclature`, `\chapterabstract`, `\chapterintisari`, `\chapterappendix`, `\addsupervisor`, `\addexaminer`. Do not redefine these.

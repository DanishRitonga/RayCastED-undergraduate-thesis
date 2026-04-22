# AGENTS.md

This repository contains a bachelor thesis written in LaTeX for DTETI UGM (Department of Electrical Engineering and Information Technology, Universitas Gadjah Mada).

## Build Commands

```bash
# Compile the full thesis (recommended)
latexmk -xelatex main.tex

# Clean build artifacts
latexmk -c

# Manual compilation sequence (if latexmk unavailable)
xelatex main.tex
bibtex main
xelatex main.tex
xelatex main.tex
```

**Note:** If compilation fails with missing `.sty` file errors, copy the required file from `packages/` to the project root.

## Architecture

**Key files:**
- `main.tex` — Master document with metadata (author, title, supervisors, examiners) and `\include{}` directives for all content
- `thesisdtetiugm.cls` — Custom document class that handles DTETI UGM formatting, cover pages, and layout
- `references.bib` — BibTeX bibliography (IEEEtran style)

**Content structure:** All thesis content lives under `contents/`:
- `abstract/` — `abstract.tex` (English) and `intisari.tex` (Indonesian)
- `chapter-1/` through `chapter-6/` — Main chapters
- `appendix/` — Appendix sections
- Front matter: `preface/`, `dedication/`, `statement/`, `nomenclature/`

**Document order in main.tex:** cover → endorsement → statement → dedication → preface → ToC/LoT/LoF → nomenclature → Indonesian abstract → English abstract → chapters 1–6 → bibliography → appendices

## Conventions

- **Document class:** `\documentclass[bachelor,english]{thesisdtetiugm}` (supports `bachelor|master|doctoral` and `bahasa|english` options)
- **Dual-language abstracts:** Required (English + Indonesian/Bahasa)
- **Figures:** Place in the corresponding chapter's directory
- **Custom commands:** The class file defines `\printcover`, `\chapterstatement`, `\chapterdedication`, `\chapterpreface`, `\chapternomenclature`, `\chapterabstract`, `\chapterintisari`, `\chapterappendix`, `\addsupervisor`, `\addexaminer` — do not redefine these
- **Template placeholders:** `sample/` contains only UGM logo and scanned document templates, not actual thesis content

## Companion Codebase

The RayCastED model implementation is in `/home/danishrtg/projects/RayCastED/`. Its authoritative specification is `docs/project.md` in that repo.

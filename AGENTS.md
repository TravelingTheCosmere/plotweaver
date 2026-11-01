# AGENTS.md

Agent notes for Steve Herron's personal fork of the unofficial Plotweaver LaTeX class (upstream `teawizardry/plotweaver`).

## Standing rules

- Personal project. Never mix in Dawson County Schools (DCS) work: no DCS files, data, names, or wording.
- Open pull requests only. Never merge, force-push, or deploy. Steve approves every merge.
- Never commit secrets, tokens, credentials, or env files.
- Steve works on Windows with PowerShell 7. Give PowerShell commands.
- Explain a risky command before you run it. That includes force-push, `git reset --hard`, `git clean`, deleting or overwriting tracked PDFs, and anything that rewrites history.
- This repo is the LaTeX class only. The Plotweaver modules core, Erstwild, Splinterlight, and Hexbeat are not in this tree, so module parity notes do not apply.

## Repository

Unofficial fan typesetting for Plotweaver RPG and Cosmere RPG, under the LaTeX Project Public License 1.3c. File headers name Hannah Kirkland as current maintainer. Not reviewed by Dragonsteel Entertainment or Brotherwise Games. Flat layout: no package manager, Makefile, CI, or test suite. LuaLaTeX is required because the class loads `fontspec`.

- `plotweaver.cls` — document class. `article` (default) or `standalone`. Color scheme option `stormlight`, `mistborn`, or `ariandor`; with none set, the class uses the Ariandor palette. Options `character-sheet`, `timeline`, and `statblock` load the matching package.
- `plotweaver-colors.sty` — palettes. `plotweaver-icons.sty` — TikZ action and plot-die icons. `plotweaver-boxes.sty` — callouts and layout boxes; the class always loads it.
- `plotweaver-mistborn.sty`, `plotweaver-stormlight.sty` — setting icons, loaded with those color schemes.
- `plotweaver-character-sheet.sty` — `\MakeCharacterSheet`. `plotweaver-timeline.sty` — timelines. `plotweaver-statblock.sty` — NPC stat blocks.
- `plotweaver.tex` — class documentation (`tcolorbox` doc commands). `example.tex` — tutorial. `character-sheet.tex` — sheet sample; portrait is `nomodan.png`.
- `plotweaver.pdf`, `example.pdf`, and `character-sheet.pdf` are tracked build output. Commit them when their sources change. `plotweaver.auxlock` is a committed TikZ externalization lock; leave it unless that setup changes.

`.gitignore` drops LaTeX intermediates (`*.aux`, `*.log`, `*.toc`, `*.out`, `*.fls`, `*.fdb_latexmk`, `*.synctex*`, `*.gz`) and `figures/`. Also ignored, so do not add them: `character_sheet_handwritten.pdf`, `character_sheet_sffamily.pdf`.

## Build and check

From the repo root. `latexmk` repeats passes so the documentation table of contents and cross-references settle.

```powershell
latexmk -lualatex -interaction=nonstopmode .\plotweaver.tex
latexmk -lualatex -interaction=nonstopmode .\example.tex
latexmk -lualatex -interaction=nonstopmode .\character-sheet.tex
```

A single manual pass:

```powershell
lualatex -interaction=nonstopmode .\example.tex
```

Fonts the class sets: TeX Gyre Schola (main), Noto Sans (sans), QT Handwriting (`\handy`), plus the `aurical` package. There is no test suite. Compile these three documents and review the PDFs.

## Conventions

- Keep the LPPL header on each source file. When files are added or removed, update the "this work consists of" list in those headers.
- New behavior goes in a package gated by a class option, same pattern as `character-sheet`, `timeline`, and `statblock`.
- A color or icon change has to stay coherent across `stormlight`, `mistborn`, and `ariandor`.
- Edit `.cls`, `.sty`, and `.tex` sources. Do not hand-edit the PDFs.

# Proposed upstream PR — nested PDF environment framing

**Target:** XimeraProject/ximeraLatex:master  
**Head:** xronosuf/ximeraLatexDev:feature/nested-pdf-styling-20261109  
**Status:** Draft; needs full class build and PDF variant check before review-ready.

## Proposed title

Add optional nesting cues for problem and theorem environments in PDF

## Proposed PR body

Introduce a `src/pdfstyling.dtx` implementation of PDF-only nested `tcolorbox` frames for theorem and problem environments. By default nesting frames are displayed in grayscale; authors may disable them by setting `\dispNestfalse`, or opt into color using `\dispNestColorstrue` before `\begin{document}`. Custom colors may be set by the author.

This feature was extracted from the previously open bundled 'problem style revamp' PR #201. Hierarchical HTML problem numbering is proposed separately.

Unlike the historical branch, deep nesting beyond the fourth color level reuses the last defined color rather than attempting to reference an undefined color name.

`src/packages.dtx` loads `tcolorbox`; the module is registered through `ximera.dtx`/`ximera.ins`, and generated `ximera.cls` is supplied. No server CSS changes or regression fixtures are included.

### Verification

The extracted DTX code matches its generated class block, and a standalone local LaTeX proof-of-concept of nested tcolorbox wrappers compiled. Full Ximera class build and on/off/color variant review are still required.

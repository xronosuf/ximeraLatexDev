# Proposed upstream PR — SagePlot HTML marker (draft for user approval)

**Target:** `XimeraProject/ximeraLatex:master`  
**Proposed head:** `xronosuf/ximeraLatexDev:feature/sageplot-inert-html-20261109`  
**Status:** Ready for user review of title/body. DO NOT open the upstream PR before user approval.

## Proposed title

Support `\sageplot` in HTML output through an optional inert Sage renderer marker

## Proposed PR body

### Summary

This adds an HTML implementation for the SageTeX `\sageplot` command in the XimeraLaTeX source (`src/interactives/sagemath.dtx`) and its generated `ximera.4ht`. For HTML output, Ximera preserves the authored Sage graphics expression in a passive `<script type="text/x-sage-plot">` inside a `.sage-plot` holder. A server that supports Sage plot rendering can recognize and replace the holder with a rendered graph; a server without that feature can ignore the nonexecuting marker.

The existing SageTeX command's PDF behavior is unchanged. The HTML override only installs when `\sageplot` is defined by SageTeX; authors are still expected to have SageTeX installed when using SagePlot. The existing SageTeX syntax with up to two optional arguments is preserved, although HTML currently ignores the presentation/format options and defers rendering to the server.

### Motivation and practical example

Sage-driven randomized graph questions may need their plots to use the same Sage generation as displayed values and validated answer keys. Xronos already has a renderer that consumes the passive marker, so the LaTeX half of this feature is useful now and can be adopted independently by additional servers later.

Published development-server activity (previously used/tested by the contributor):

https://dev.xronos.clas.ufl.edu/testsuite/test-suite-xourse/testSuite/14-sageplot-svg

Corresponding author source:

https://github.com/xronosuf/testFiles/blob/main/testSuite/14-sageplot-svg.tex

This activity contains two graphs, values randomized with Sage, associated answer boxes, and alt descriptions. The working LaTeX override was recovered from:

https://github.com/xronosuf/testFiles/blob/main/.ximera_local/src/interactives/sagemath.dtx

### Compatibility and scope

- This PR updates only XimeraLaTeX. It **does not** change the Ximera server, introduce a Sage service, or require that every deployment support Sage.
- The marker uses a non-JavaScript script type, so unsupported servers have no SagePlot client-code dependency and do not execute the expression. Unsupported servers will not display a plot; this is an intentional no-op, **not** a graph fallback.
- Normal PDF processing remains the responsibility of SageTeX/Sage.
- HTML arguments have SageTeX-compatible parsing and retain the authored expression for the existing Xronos renderer.

### Verification / review notes

- Compared the new `.dtx` implementation with the local version used by `xronosuf/testFiles`; the emitted HTML marker and expression are equivalent. The internal helper name and an unused optional-argument placeholder differ.
- Verified the new `src/interactives/sagemath.dtx` snippet is synchronized with the corresponding generated `ximera.4ht` snippet.
- Verified the feature branch differs from the clean upstream baseline **only** in those two files. Regression fixtures live in the separate test-suite repository, not in this source PR.
- The contributor reports the above development-server activity was previously published and tested. This draft does **not** claim that a fresh browser or full upstream compilation test was executed as part of this recovery.

### Follow-up, deliberately out of scope

Supporting server implementations may be contributed separately. The existing Xronos server code is not part of this PR.

---

**Review-only metadata:** This draft lives on the development fork's review branch, not on the feature PR head. No upstream PR has been opened.
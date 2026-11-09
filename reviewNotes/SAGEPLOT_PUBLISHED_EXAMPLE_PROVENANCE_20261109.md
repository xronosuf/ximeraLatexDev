# SagePlot published-test evidence and upstream port comparison — 2026-11-09

Review-only record, not for inclusion in any upstream feature PR.

## User-supplied published reference

- https://dev.xronos.clas.ufl.edu/testsuite/test-suite-xourse/testSuite/14-sageplot-svg
- User reports this activity was published and tested successfully on the Xronos development server. Browser retrieval failed with a cache miss; direct container HTTP access failed due to DNS resolution. Treat live runtime behavior as USER-REPORTED, not independently re-tested.

## GitHub source and publishing provenance

- Xourse: https://github.com/xronosuf/testFiles/blob/main/test-suite-xourse.tex contains `\activity{testSuite/14-sageplot-svg}`.
- Test source: https://github.com/xronosuf/testFiles/blob/main/testSuite/14-sageplot-svg.tex has two `\sageplot{plot(...)}` calls, `\sagesilent` randomized parameters, `\sage` inline values, answer fields, `\xmalt` descriptions, and listed manual checks for SVG rendering, generation/reload consistency, and Show Me Another.
- Publishing configuration: https://github.com/xronosuf/testFiles/blob/main/xmScripts/config.txt sets defaults `XIMERA_NAME=testsuite` and `XIMERA_URL=https://dev.xronos.clas.ufl.edu/`; this independently matches the supplied URL's course/path structure, but does not prove the specific deployed version.
- Original XimeraLaTeX override: https://github.com/xronosuf/testFiles/blob/main/.ximera_local/src/interactives/sagemath.dtx
- Original generated HTML configuration: https://github.com/xronosuf/testFiles/blob/main/.ximera_local/ximera.4ht contains matching SagePlot override.
- Existing compatible Xronos renderer: https://github.com/xronosuf/server/blob/production/public/javascripts/sageplot-bootstrap.js matches `script[type="text/x-sage-plot"]` and `.sage-plot` parent markers.

## Comparison to proposed upstream contribution

- Branch: https://github.com/xronosuf/ximeraLatexDev/tree/feature/sageplot-inert-html-20261109, originating from clean `base/upstream-master` commit `590672174f137a913598f78e38df831ec23ff5ba`.
- `src/interactives/sagemath.dtx` in feature branch keeps the tested implementation's `\ifdefined\sageplot` guard, `\renewcommand` wrapper, two-option SageTeX grammar, inert `<span class="sage-plot"><script type="text/x-sage-plot">` and `\HCode{\detokenize{#3}}` content.
- The only SagePlot macro-code differences observed against testFiles are an internal helper rename (`\xm@sageplot` to `\xm@sageplot@html`) and the missing second optional argument default (`[notprovided]` to `[]`). That argument is not emitted or used by the HTML renderer, so both variants produce identical HTML marker output for the same expression.
- The feature branch's `.dtx` override and corresponding `ximera.4ht` output were checked for source/generated consistency. The generated `ximera.cls` is identical to upstream; no SageTeX-missing fallback is included.
- Comparison against `base/upstream-master` showed only `src/interactives/sagemath.dtx` and `ximera.4ht` modified. No regression fixtures are in the feature branch; all regression tests belong elsewhere per user decision.

## Intended deployment contract

- Xronos has existing SagePlot runtime support and can act on these passive markers.
- Servers without SagePlot runtime do not execute scripts of the non-JavaScript marker type; compilation itself does not require a server modification. Missing SageTeX is an author-side dependency error if `\sageplot` is used, per user's explicit decision.
- The source and provenance support treating this as a practical implementation rather than a speculative design. This review does NOT substitute for independently loading the user's deployed page, compiling the full upstream branch, or acceptance testing by an unsupported server.

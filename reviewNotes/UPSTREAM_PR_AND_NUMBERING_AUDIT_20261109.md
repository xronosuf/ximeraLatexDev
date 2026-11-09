# Upstream PR overlap and numbering trace — 2026-11-09

**Investigation only.** Keep this file exclusively on the `review/ximera-recovery-inventory-20261109` branch of `xronosuf/ximeraLatexDev`. No edits to other repositories and no feature implementations or upstream PRs are authorized by this note.

## Repository baseline
- `XimeraProject/ximeraLatex:master` at `590672174f137a913598f78e38df831ec23ff5ba` (2026-04-10).
- Fork `base/upstream-master` matches that commit. Fork master is four commits ahead, but its tree difference against the baseline consists of two new workflow-reference text files.
- Review branch diverges from the historical base; do **not** merge review notes into upstream-targeted feature branches.

## CRITICAL: existing upstream PR #201
https://github.com/XimeraProject/ximeraLatex/pull/201

As checked 2026-11-09, this original `problem style revamp` PR is OPEN, unmerged, **mergeable=false** (`dirty`), with head `XimeraProject/ximeraLatex:jason-problemStyleRevamp` at `e3d29f61daaf23dba97aa8137fd1b07c8c805d87`. It combines two independent proposals:
- `src/pdfstyling.dtx`, `src/packages.dtx`, plus registration/build changes: nested PDF box styling.
- `src/problem.dtx` plus generated `ximera.4ht`: hierarchical online problem numbering.
- Generated `ximera.cls`, PDF and build artifacts are also included.

Historical review comments by `wiobber` (June 23–24, 2025) questioned the inverted conditional, a commented-out branch, and missing `global.css` context. The author replied to each question and explained the conditional, noted a temporary debug comment, and said the CSS was in examples/test sources. Preserve those review discussions as provenance. Because user wants one feature per PR, do not simply re-submit the combined old patch or edit/close this upstream PR without a decision. Instead extract and modernize two clean fork-side feature branches after inventory approval, reference #201 when submitting, and coordinate supersession.

## C04b: static numbering source/renderer trace

1. Current upstream `src/options.dtx` defines `\ifnumberedProblems` defaulting false and the `onlineProblemNumbers` class option. Yet the HTML counting block in `src/problem.dtx` is commented out. `\problemNumDisp` is empty, and generated `ximera.4ht` emits `numbered=" \problemNumDisp"`.
2. Old PR #201 `src/problem.dtx` conditionally creates nested numeric values, but emits `probnum="\probNumDisp"` rather than `numbered`. Its display string covers only I–V levels, even though its counter creation can reach deeper levels. The `noNumberedProblems` inverted conditional is intentional in historical review, but needs independent technical testing.
3. The inspected Xronos server production `public/stylesheets/theorems.scss` and `public/stylesheets/base.css` consume `attr(numbered)`. So do XimeraProject/server styling and Ximera FirstSteps CSS. The old `probnum` attribute **does not match** this established contract.
4. `mac1114/main/global.css` and `mac2233/main/global.css` contain identical problem-numbering CSS, notably `.problem-environment[titletext]::before { content:attr(titletext) attr(numbered) "."; }`. The stylesheet alone does not explain reported different behavior.
5. MAC1114 sample `anglesAndCircles/Tile1.tex` contains actual nested `problem` environments; MAC2233 sample `Limits/introduction/geometricView.tex` contains flat `problem`s plus a theorem-like `example`. Neither sampled `\documentclass{ximera}` requests `onlineProblemNumbers`; the MAC2233 xourse also omits the option. Theorem labels and problem numbering must be distinguished.
6. Repository configs select different XAKE images (`v2.7.9-full` for MAC1114, `v2.6.8-full` for MAC2233), and the repositories contain tag-triggered publishing workflows with environment variable overrides. This is a possible source of version drift, **not evidence** of which build actually produced deployed HTML.
7. Four plausible Xronos live/test activity URLs were not accessible to the browser. There were no accessible workflow runs in GitHub's returned collection for either course. **No live HTML behavior or deployed build provenance has been verified.**

### Tests required before a numbering PR
- Preserve default unnumbered HTML; verify both class option and user preamble toggle.
- Emit HTML `numbered` (not `probnum`) under existing server/FirstSteps CSS.
- Check siblings, children, parent return, repeated depth resetting and nesting beyond five levels.
- Verify no side effects on theorem-like `example`, `exercise`, `question`, `hint` behavior.
- Verify PDF numbering independently; PDF `\problemNumber` exists upstream already.
- Regenerate class and 4ht from DTX and test generated output, not historical generated artifacts.

## Other upstream PR overlap
- PR #272, OPEN and targeting `development`: https://github.com/XimeraProject/ximeraLatex/pull/272. Its description discusses footnote accessibility, **but its current 35-file changed-file list does not include `src/footnotes.dtx`**. Its `ximera.4ht` patch changes theorem markup rather than footnotes. Keep C07 footnote candidate pending a later recheck rather than presuming it landed.
- PR #272 DOES modify `src/validators/factorCheck.dtx` to adjust Math Expressions handling and CDATA; it does not include the contextual DOM status helper from `ValidatorExamples/pageManipulationValidators/advancedFeedback.tex`. C03 remains a distinct proposed feature, with possible integration conflict if #272 lands.
- Other currently open PRs: #271 Doenet URL, #270 HTML-only package block, #269 package blocking, #265 headings. None of these changes is merged into current master.

## Updated planned upstream PR boundaries (subject to user inventory review)

| ID | Scope | Status |
| --- | --- | --- |
| C01 | HaLLMos DTX command/env, HTML embed, PDF fallback | Reconstruct from generated-only history, test classroom example |
| C02 | SagePlot HTML DTX and explicit Xronos server dependency | Port source, regenerate and test |
| C03 | Contextual answer feedback, validator hooks and required generic DOM helper | Design/port/test as one coherent feature, monitor #272 overlap |
| C04a | Nested PDF styling only | **Hold** pending switch/nesting tests; separate from #201 |
| C04b | Optional hierarchical HTML problem numbering only | **Keep conditionally**; repair contract, tests; separate from #201 |
| C06 | Two development workflow reference documents | Independent docs-only PR, content exists in fork master |
| C07 | Expandable footnote bug fix | Check historical corrections with current code, monitor #272 |
| C05, C08 | Course styling/QR, xmsection aliases | Excluded by user decision |

**Stage gate:** show user consolidated inventory/PR boundaries before feature implementation or submission. Do not touch/close #201 without clear authorization. Do not make live deployment claims until verified.

## Links
- https://github.com/XimeraProject/ximeraLatex/pull/201
- https://github.com/XimeraProject/ximeraLatex/pull/272
- https://github.com/XimeraProject/ximeraLatex/blob/master/src/problem.dtx
- https://github.com/XimeraProject/ximeraLatex/blob/master/src/options.dtx
- https://github.com/xronosuf/ximeraLatexDev/blob/jason-problemStyleRevamp/src/problem.dtx
- https://github.com/xronosuf/server/blob/production/public/stylesheets/theorems.scss
- https://github.com/xronosuf/mac1114/blob/main/global.css
- https://github.com/xronosuf/mac2233/blob/main/global.css

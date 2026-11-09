# XimeraLaTeX contribution readiness — November 9, 2026

All feature branches are based on `base/upstream-master` SHA `590672174f137a913598f78e38df831ec23ff5ba` (matching XimeraProject/ximeraLatex:master). They exclude master-only workflow docs and review notes; documentation has its own branch.

## C02 — SagePlot: submitted to upstream as PR #277
- Branch: https://github.com/xronosuf/ximeraLatexDev/tree/feature/sageplot-inert-html-20261109
- PR text: reviewNotes/PR_DRAFT_C02_SAGEPLOT.md
- Diff: `src/interactives/sagemath.dtx`, generated `ximera.4ht` only. Source/generated block verified equivalent, plus code matches original tested testFiles implementation and Xronos renderer contract.
- User approved title and description, then submitted the PR through GitHub: https://github.com/XimeraProject/ximeraLatex/pull/277. GitHub confirmed it is open, targeting master, with exactly the DTX source and generated 4ht changed; the earlier connector-side submission attempt had failed 403.
- User-supplied deployed activity: https://dev.xronos.clas.ufl.edu/testsuite/test-suite-xourse/testSuite/14-sageplot-svg. Content's GitHub source verified; current browser load not verified.

## C01 — HaLLMos: source and PR draft prepared; full build/live remote behavior unverified
- Branch: https://github.com/xronosuf/ximeraLatexDev/tree/feature/hallmos-embed-20261109
- PR text: reviewNotes/PR_DRAFT_C01_HALLMOS.md
- Files: `src/interactives/hallmos.dtx`, `ximera.dtx`, `ximera.ins`, generated `ximera.cls`, `ximera.4ht`.
- Generated class and HTML fragments match DTX. The MAA4102 local class is byte-identical to the historical HaLLMos demo class, and their HTML HaLLMos markup is equivalent. A local PDF smoke compiled the actual MAA4102 statement; a local TeX4ht smoke emitted the script/figure/blockquote/math markup, though the old installed Ximera toolchain separately reported unrelated configuration errors. See reviewNotes/HALLMOS_MAA4102_VERIFICATION_20261109.md. PDF outputs statement rather than historical placeholder. Optional metadata is still ignored.
- User-reported deployed activity: https://xronos.clas.ufl.edu/maa4102/Homework1/p05; GitHub author source verified. HTML/PDF full build verification remains.

## C04b — Hierarchical numbering: source and PR draft prepared; behavior tests external
- Branch: https://github.com/xronosuf/ximeraLatexDev/tree/feature/hierarchical-problem-numbering-20261109
- PR text: reviewNotes/PR_DRAFT_C04B_NUMBERING.md
- Diff only `src/problem.dtx` plus generated `ximera.4ht`. HTML DTX block verified against generated block, and legacy `probnum` attribute corrected to existing `numbered` CSS contract.
- Preserves historical nested-counter method and default-off `onlineProblemNumbers` class option. Full Ximera HTML compile/runtime, nested-depth behavior, and numbering opt-in still should be checked in external regression workflow.
- Prior open upstream PR #201 contains old combined styling/numbering patch. No edit or closure of #201 was attempted.

## C06 — Workflow reference docs: ready for user PR review
- Branch: https://github.com/xronosuf/ximeraLatexDev/tree/docs/validator-workflow-reference-20261109
- PR text: reviewNotes/PR_DRAFT_C06_WORKFLOW_DOCS.md
- Only two `usefulReferences/workflowWorkthroughs` files differ against upstream. Warns strongly grouped validators are broken/not recommended for new student materials. No generated artifacts or tests are applicable.

## C04a — Nested PDF styling: source and PR draft prepared; full class build outstanding
- Branch: https://github.com/xronosuf/ximeraLatexDev/tree/feature/nested-pdf-styling-20261109
- PR text: reviewNotes/PR_DRAFT_C04A_PDF_STYLING.md
- Files: new `src/pdfstyling.dtx`, updated packages/registrations, generated `ximera.cls`. The DTX class block is verified present identically in class.
- Local standalone pdflatex checks compiled in all four cases: default nested environment frames, five-level deep nesting, optional color, and disabling frames. These are **component checks, not a full Ximera build**. No test files were committed.
- Does not include numbering changes; deep frames clamp colors after level IV. Corrected a historical omission so the literal `theorem` environment receives a frame. Local TeX Live test compiled a nested theorem/example/theorem series with the plain `tcolorbox` package. External full modern Ximera build and visual review pending.

## C07 — Expandable footnotes: source and PR draft prepared; browser behavior check outstanding
- Branch: https://github.com/xronosuf/ximeraLatexDev/tree/feature/expandable-footnotes-20261109
- PR text: reviewNotes/PR_DRAFT_C07_FOOTNOTES.md
- Diff only `src/footnotes.dtx` plus generated `ximera.4ht`. The source and generated blocks match exactly after normalization.
- Fixes malformed CSS style syntax (including the formerly missed `vertical-align=baseline`), removes the old fixed blue wrapper color, adds button ARIA state/controls and a safer JS toggle. Local TeX4ht output smoke generated correctly linked footnote controls for two notes, and the generated JS passed Node syntax check. The old installed class emits separate unrelated TeX4ht errors, so a modern clean build and browser/accessibility checks are still pending. Open PR #272 mentions footnotes in its description but did not include footnotes.dtx in its retrieved changed-file list.

## C03 — Dynamic validator feedback: PENDING, stop implementation
- Deferred at user request. Original material at https://github.com/xronosuf/ValidatorExamples/blob/main/pageManipulationValidators/advancedFeedback.tex and https://github.com/xronosuf/ValidatorExamples/blob/main/.ximera_local/src/validators/factorCheck.dtx.
- Resume note: reviewNotes/C03_DYNAMIC_FEEDBACK_PENDING.md. Empty branch `feature/dynamic-validator-feedback-20261109` is a starting pointer, not a feature.

## Explicit exclusions
- C05 personal course PDF/QR styling and C08 old xmsection aliases remain out of scope.

## Submission safety
- User reviewed and approved SagePlot PR text and submitted upstream PR #277 manually. Do not submit any other PRs without review. GitHub connector previously returned 403 creating an upstream PR; other feature work remains in the fork.
- Full upstream build / browser tests were not claimed except local component checks. Functional regression tests belong in external test suite, NOT in upstream feature branches.

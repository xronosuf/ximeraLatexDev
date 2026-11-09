# XimeraLaTeX recovery inventory — discovery working notes

Started: 2026-11-09. STATUS: INITIAL SOURCE / FEATURE TRIAGE, NOT A FINAL OR PR-READY INVENTORY.

## Scope and safety
- All repositories other than `xronosuf/ximeraLatexDev` are STRICTLY READ-ONLY. No writes or temporary branches permitted without fresh explicit authorization.
- `xronosuf/ximeraLatexDev:base/upstream-master` at `590672174f137a913598f78e38df831ec23ff5ba` was byte/commit-identical to upstream when checked. Preserve as clean reference.
- Inventory and PR drafting belong on review branches, never final feature branches. Every new feature gets its own independent PR. Group only closely related bug/efficiency fixes.
- Generated `.cls`, `.4ht`, `.cfg`, and scripts are historical behavioral evidence; assess whether `.dtx` matches. Avoid attributing generic changes between 2025 and 2026 to the user.

## Discovery coverage
- Accessible repositories: 30 under `xronosuf` and 2 under `Zengez` (mathDef, mathThm). Every default-branch recursive tree was inspected for `.ximera_local`, `ximera_local-BAK`, standalone Ximera class/config/build files, `.dtx`, HaLLMos and SagePlot. Git tree responses used were not truncated.
- Also inspected alternate branches for calculus, map2302text, ximeraFirstSteps, mac2311, mac1140, mac2313, testFiles, components, HaLLMos-embed, mac1105, ximeraLatexDev and principal server/server-1 development branches. Automated Dependabot branches and mathDef/mathThm side branches are not yet deeply screened.
- Found committed `.ximera_local` trees in mac2311/main, mac1105/main and master, mac1147/main, mac2312/main, maa4102/main, ValidatorExamples/main, testFiles/main, HaLLMos-embed/master, mac1140/originalContent and selected newer mac1140 branches. The mac1140 default instead carries `ximera_local-BAK/`.
- Also found non-`.ximera_local` legacy generated files in `TestBed/Styling/ximera.cls` and `ximeraFirstSteps` OLDmain/wim_test-ku-style under `.ximera/`, plus an independent package architecture in `components`.
- `mac2311/main`, `mac1105/main`, `mac1147/main`, `mac2312/main`, and `mac1140/originalContent` have identical 101-file `.ximera_local` snapshot content; the 101-file `ximera_local-BAK` on `mac1140/main` also matches. Thus these are repeated evidence, not six separate features.

## Distinct contribution candidates — PRELIMINARY

### C01 — HaLLMos embed (standalone feature)
- Evidence: `xronosuf/HaLLMos-embed:master/.ximera_local/ximera.cls` and `ximera.4ht`; also found equivalent HaLLMos compiled edits in `xronosuf/maa4102:main/.ximera_local/`.
- Macros: `\hallmos` command plus `hallmosEnv` environment; HTML loads `https://hallmos.com/embed.js` and wraps supplied statement in a `.hallmos-embed` figure/blockquote.
- Source status: GENERATED ONLY in inspected snapshots (no matching HaLLMos `.dtx` found). Reconstruct a standalone `.dtx` and registration/build changes against modern upstream.
- Important discrepancy: PDF class command takes one mandatory argument, while HTML command accepts an optional plus a mandatory argument; PDF environment is placeholder text. Need reconcile semantics, PDF fallback, optional instructor-note handling, script duplication, accessibility, and compatibility.
- Practice source: https://github.com/xronosuf/HaLLMos-embed/blob/master/HaLLMos.tex and https://github.com/xronosuf/HaLLMos-embed/blob/master/hallmosXourse.tex ; production URL not confirmed.
- Preserve as its own feature PR, not bundled with Doenet or SagePlot.

### C02 — SagePlot HTML output (standalone feature; server dependency)
- Evidence: `xronosuf/testFiles:main/.ximera_local/src/interactives/sagemath.dtx` and generated `ximera.4ht` include `\sageplot` support; corresponding upstream `src/interactives/sagemath.dtx` lacks it.
- HTML emits `.sage-plot` marker and `script[type="text/x-sage-plot"]` containing authored Sage expression. SageTeX retains the PDF behavior.
- Source status: matching `.dtx` and generated HTML implementation observed; needs rebuild/verification in current upstream.
- Server interaction: `xronosuf/server:production/documentation/SAGEPLOT.md`, `public/javascripts/sageplot-bootstrap.js`, canonical seeded Sage renderer / SVG; `sageplot` server branch also exists but is historically divergent. Need separate dependency documentation and validation.
- Practice source: https://github.com/xronosuf/testFiles/blob/main/testSuite/14-sageplot-svg.tex ; test page publication mentioned in docs, exact public URL unconfirmed.

### C03 — Dynamic validator-specific feedback (candidate standalone feature)
- Evidence: `xronosuf/ValidatorExamples:main/.ximera_local/src/validators/factorCheck.dtx` adds calls to `displayMessageBelowCurrentAnswer`; matching generated `ximera.cls` was changed in the same 2026-05-28 commit. Upstream/current `factorCheck.dtx` does not contain these calls.
- Demonstration defines contextual feedback helper in authored `\begin{javascript}` with a `role=status`, `aria-live=polite` message and several validator examples.
- Practice source: https://github.com/xronosuf/ValidatorExamples/blob/main/pageManipulationValidators/advancedFeedback.tex ; also `advancedValidation/factorCheck.tex`.
- Source status: local source and generated implementation both altered, but browser helper currently lives in page-authored JavaScript; decide whether generic runtime support and validator-specific messages should be separate dependent proposals or one coherent feature. Behavior not tested.

### C04 — Nested PDF theorem/problem styling (candidate standalone feature)
- Evidence: `xronosuf/ximeraLatexDev:jason-problemStyleRevamp` adds `src/pdfstyling.dtx`, `tcolorbox` package use, configurable nested color/black box outlines for theorem/problem environments.
- Also changes `src/problem.dtx` numbering and HTML configuration. These may be separable from the PDF styling feature; inspect final desired semantics before defining PR boundaries.
- Source and generated files (`ximera.cls`, `ximera.4ht`) were both edited on that branch, so regenerate and compare before claiming consistency. Current branch substantially predates modern upstream.
- Practice example not yet confirmed.

### C05 — Legacy PDF layout/interactive placeholders (candidate standalone feature, possibly multiple separable capabilities)
- Evidence: `xronosuf/mac2233:main/Limits/ximerapdfrevamp.sty` (course-local `.sty`, no matching `.dtx`).
- Implements configurable tcolorbox presentation for examples, definitions, theorems, problems; PDF YouTube/Desmos fallbacks with QR codes; and print formatting of choice answers.
- Determine whether this is merely course-specific styling or a generally desirable Ximera option. Do not silently merge into C04; overlapping motivation, different behavior. Separate distinct capabilities if chosen.
- Source: https://github.com/xronosuf/mac2233/blob/main/Limits/ximerapdfrevamp.sty

### C06 — Detailed workflow references (documentation-only independent PR)
- `xronosuf/ximeraLatexDev:master` differs from upstream only by `usefulReferences/workflowWorkthroughs/01-individual-answer-validator.txt` and `02-grouped-validator-environment.txt`.
- This is a standalone documentation contribution; user wants it included as its own PR, not automatically submitted now.

## Investigated historical differences already upstream / not presumed PR material
- `maa4102` generated files contain hint capitalization, author footnote presentation, updated YouTube iframe output, and a `freeResponse` paragraph fix. Direct checks of current upstream `ximera.cls`, `ximera.4ht`, `src/hints.dtx`, `src/title.dtx` found those same behaviors already upstream. Do NOT create redundant PRs.
- `ValidatorExamples` has an older `factorCheck.dtx` augmented with dynamic feedback; the ordinary upstream validator implementation otherwise matches `testFiles`. Its extra messages are NOT upstream and are tracked as C03.
- `ximeraLatexDev:draft-restructuredDTX` holds experimental reorganizations under `src.Option1-MergedDTX` and `src.Option2-FolderStructureDTX`, explicitly described as proof-of-concept and not wired into live build. Review as architectural reference, not a finished independent feature.
- `ximeraLatexDev:222-a11y` mainly concerns Docker image workflows and dependency changes; verify whether any uniquely useful build fix remains.
- `xronosuf/components` is a different `tex4npm` component monorepo architecture, not a drop-in `.dtx` feature; don't treat its many component copies as independent features without evidence of actual new behavior.
- `xronosuf/ximeraDevJason` README proposes answer-box accessibility and `\intent` but the inspected default tree contains no `.cls/.4ht/.dtx` implementation. Mark as design-only, not implemented.
- `xronosuf/TestBed:master/Styling/ximera.cls` is a legacy 2020 copy; its commit history indicates an unsuccessful test of whether local CLS loaded. No clear portable feature established.
- `xronosuf/ximeraFirstSteps` old branches contain `.ximera/ximera.cfg` and sometimes `.cls`, with no `.dtx` in those branches. Attribution/functional differences vs contemporary upstream not established.

## Remaining checks before declaring inventory complete
- Finish nondefault / historical commit coverage for relevant repositories, including potentially deleted `.ximera_local` variants; verify there are no other generated-file-only modifications.
- Inspect code-level changes in older `jason-problemStyleRevamp`, legacy `.ximera` copies, and any `.sty`/preamble overrides indicating a reusable package feature.
- Search published/deployed examples and record a verified link for EACH feature PR; a GitHub `.tex` file is evidence but not a verified deployed demo.
- Classify every candidate as already-upstream, ready to port, requires modernization, design-only, or uncertain; separately record `.dtx` source-consistency status and dependencies.
- Prepare user checkpoint BEFORE any feature implementation or upstream PR submission. No feature PRs have been created.

## Source review URLs
- https://github.com/xronosuf/HaLLMos-embed/tree/master
- https://github.com/xronosuf/testFiles/tree/main
- https://github.com/xronosuf/ValidatorExamples/tree/main
- https://github.com/xronosuf/maa4102/tree/main
- https://github.com/xronosuf/ximeraLatexDev/tree/jason-problemStyleRevamp
- https://github.com/xronosuf/ximeraLatexDev/tree/draft-restructuredDTX
- https://github.com/xronosuf/components/tree/main

Investigation-only record. Never merge this file into upstream or feature PR branches.

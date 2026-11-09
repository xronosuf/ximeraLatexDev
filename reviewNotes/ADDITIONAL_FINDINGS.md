# Recovery investigation — follow-up findings and validation notes

Date: 2026-11-09. This is an investigation record only. **NOT** an upstream PR artifact and **NOT** a finalized feature list.

## C07 — HTML expandable-footnote markup correction (candidate bug-fix PR)

- Source: `xronosuf/ximeraLatexDev:jason-problemStyleRevamp/src/footnotes.dtx` and corresponding generated `ximera.4ht`, compared with `development` and `base/upstream-master`.
- The historical branch has modifications to the JavaScript function (`footnotefunction` rather than `footnotefun`), brace/`<script>` boundary, inline style syntax, button onclick quotation, and embedded superscript/button closure.
- Current upstream `src/footnotes.dtx` is **identical** to the older `development` snapshot for this file (blob `1ad7038...`), whereas the proposed revised version is blob `cd37a08...`; thus this is not already incorporated as an identical source patch.
- Potential purpose: fix parsing or generated HTML correctness of expandable footnotes. **Not yet validated**: the old branch may have been experimental, and actual behavior must be checked in modern build. Do not bundle with nested PDF boxes merely because both lived on one historical branch.
- Note: a GitHub compare between widely divergent branch tips lists commits' relative change paths and may omit changes introduced on the opposite historical line. Full tree/blob comparisons expose the actual `src/footnotes.dtx` difference.

## C08 — Legacy sectioning aliases `\\xmsection` and `\\xmsubsection` (uncertain / other contributor)

- Source: `xronosuf/ximeraFirstSteps:wim_test-ku-style/.ximera/ximera.cls` at roughly lines 1053-1054, defining `\\providecommand\\xmsection\\section` and `\\providecommand\\xmsubsection\\subsection`; matching `KUStyle/preamble.tex` redefines defaults to `\\subsection` and `\\subsubsection`.
- `xronosuf/ximeraFirstSteps:OLDmain/.ximera/ximera.cfg` is an older standalone copy. No corresponding local `.dtx` source exists in that branch.
- The class file was added in commit `2737b2df4294` dated 2024-08-22, titled "preamble/ximera.cls: add \\xmsection". Current upstream generated `ximera.cls` did **not** contain those aliases on direct text inspection.
- Branch is named `wim_test-ku-style`; **authorship and intention uncertain**, and aliases may be KU-specific styling rather than a generally portable new XimeraLaTeX feature. Don't attribute to user or propose PR without review.

## C04 split possibility — Nested problem numbering, distinct from nested PDF styling

- `xronosuf/ximeraLatexDev:jason-problemStyleRevamp/src/problem.dtx` adds generated hierarchical problem numbering in HTML (`probnum` attribute, depth counters, conditionally numbered problems), while `src/pdfstyling.dtx` independently adds colored/black nested borders to PDF environments.
- These have different purposes and may warrant separate feature PRs, respecting the rule one new feature per PR. We need inspect dependencies and any server renderer support for `probnum`.
- `src/pdfstyling.dtx` uses a `tcolorbox` package addition. Both code and generated files exist, but regeneration/testing needed.
- The old branch also changes `src/interactives/google.dtx` to simplify Spreadsheet links; unclear whether deliberate fix; do not port automatically.
- It modifies `src/title.dtx` and `xmScripts/xmlatex` in ways that may be old-version reversions rather than improvements; modern upstream already supports revised author footnote presentation and newer key-management changes. Avoid copying these incidentals.

## C05 clarification — Course-local PDF style is not demonstrated active

- `xronosuf/mac2233:main/Limits/ximerapdfrevamp.sty` includes configurable PDF tcolorbox styling, QR-code links for Desmos/YouTube, and custom PDF `\\choice` presentation.
- In the inspected `Limits/preamble.tex`, the line `%\\usepackage{ximerapdfrevamp}` is **commented out**. Thus this is candidate source code, not evidence it is enabled for the course. Check history/usage before claiming live deployment.
- Possibly split QR-code print fallbacks from environment styling, because the behaviors are independent.

## C03 clarification — Validator feedback requires generic runtime exposure

- `ValidatorExamples/pageManipulationValidators/advancedFeedback.tex` defines its own JavaScript `displayMessageBelowCurrentAnswer` that tracks focused/clicked answer and inserts DOM status messages via `role=status` and `aria-live=polite`.
- `ValidatorExamples/.ximera_local/src/validators/factorCheck.dtx` has multiple guarded calls to this function. Unless helper is present on a page, the validator additions alone do not display any extra feedback. Treat both halves as one coherent proposed capability or explicitly document dependency. The runtime helper is currently in the example page rather than a published generic Ximera module.
- Do not assert this is accessibility-tested merely because it uses ARIA attributes.

## Historical source discrimination

- Several course local `.sty` files are third-party or subject-specific utilities (e.g. `MAP2302/mywrapfig.sty`, `tasks.sty`, `mac1140/longdivision.sty`), not automatically upstream Ximera features.
- `xronosuf/TestBed/master/Styling/ximera.cls` is a 2020 local generated copy, with commit notes explicitly saying the test picked up remote Ohio State classes. `Styling/problemStyling.tex` contains a largely commented-out customization prototype, rather than evidence of working Ximera package change.
- `xronosuf/ximeraDevJason:main/README.md` discusses accessibility and proposed `\\intent` behavior but has no default-branch implementation; retain as design-only provenance.
- `xronosuf/ximeraLatexDev:draft-restructuredDTX` README says proof of concept; specifically records preference for separate small DTX files organized in folders. Not a user-approved upstream feature as-is.
- `xronosuf/components` is a distinct `tex4npm` component architecture. Treat as related development context, not 10+ plug-and-play PRs to the original `ximeraLatex` project.

## Required checkpoints

1. Verify any candidate missing from historical branch tips/deleted copies before asserting full recovery.
2. Verify a live or test page for each new feature; source code links alone are not deployed demonstrations.
3. Show user the inventory, uncertainties, source / generated discrepancies and proposed one-feature-per-PR splits before reconstructing feature code.
4. Do not write to any repository except `xronosuf/ximeraLatexDev` and do not create upstream PRs yet.

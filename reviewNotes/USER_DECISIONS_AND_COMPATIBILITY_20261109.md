# User decisions and updated investigation directions (2026-11-09)

**Status:** Discovery/classification guidance, review-only. Do not copy this file into any upstream feature PR. User has given the go-ahead to continue investigation, NOT blanket authorization to submit PRs or edit any historical source repository.

## Decisions and primary candidate status

| ID | Candidate | Decision | Next action |
| --- | --- | --- | --- |
| C01 | HaLLMos embedding | **KEEP, independent feature PR** | Reconstruct the working command/environment from generated class/4ht code into DTX; retain real classroom demonstration; test PDF fallback and HTML embed. |
| C02 | SagePlot | **KEEP as standalone feature candidate** (previously discussed) | Port existing DTX/HTML code, document server dependency and seeded SVG results, validate with testSuite/14. |
| C03 | Dynamic validator feedback | **KEEP, important standalone feature** | Implement in the existing validator infrastructure and DTX folder. Account for source-page JS helper as supporting implementation. Verify examples and whether generic API should be loaded automatically. |
| C04a | Nested PDF theorem/problem styling | **HOLD AS POSSIBLY PARTIAL FEATURE, don't submit as-is** | Preserve working research/review branch; test both on/off and color on/off switches and nested behavior; determine if further work is needed before PR. |
| C04b | Hierarchical problem numbering | **KEEP CONDITIONALLY** | Check original feature intent: author-selectable numbers versus none, possibly alternative formats; verify modern class + CSS compatibility; one feature PR if behavior is recoverable. |
| C05 | Personal course PDF styling/QR fallbacks | **EXCLUDE FROM UPSTREAM PR SCOPE FOR NOW** | Retain only source reference in historical notes; no port or PR. |
| C06 | Development workflow reference docs | **KEEP, separate documentation-only PR** | Prepare clean documentation PR later; note maintainers may prefer another place for specialized developer docs. |
| C07 | Expandable-footnote correction | **KEEP as bug-fix candidate** | Reconstruct the corrected DTX/4ht pair and validate generated HTML; keep separate from styling. |
| C08 | Legacy xmsection/xmsubsection aliases | **DROP FROM PR CANDIDATES** | Current XimeraProject ximeraLatex and ximeraFirstSteps default branches yielded no xmsection source; retain minimal discovery provenance only. |

## Confirmed HaLLMos classroom use

User supplied these deployed links and confirmed they were used successfully in an analysis course:

- Specific activity: https://xronos.clas.ufl.edu/maa4102/Homework1/p05
- Course homepage / additional activities: https://xronos.clas.ufl.edu/maa4102

Actual source found: https://github.com/xronosuf/maa4102/blob/main/Homework1/p05.tex, which uses:
\`\`\`latex
\begin{hallmosEnv}[]
  Let $x$ and $y$ be real numbers, and $n$ be a positive integer. Then
  % difference-of-powers identity ...
\end{hallmosEnv}
\`\`\`

These are **user-verified deployed examples** with a confirmed matching source file. The published pages did not load in the automated browser (cache miss); **do not claim independent live testing**. Demo links belong in the eventual HaLLMos PR description once verified as publicly viewable by reviewers. All \`maa4102\` files remain read-only.

## Dynamic feedback: technical notes

The Ximera source already provides \`src/validators/validatorHook.dtx\` for stable validator JavaScript and \`src/answer.dtx\` supports \`validator=...\` on \`\\answer\`. Existing permanent validators live in \`src/validators/\`.

In \`xronosuf/ValidatorExamples:main/.ximera_local/src/validators/factorCheck.dtx\`, extra guarded calls to \`displayMessageBelowCurrentAnswer(text)\` provide targeted explanatory messages. The helper is supplied in \`pageManipulationValidators/advancedFeedback.tex\` (tracks active answer/problem, inserts \`.custom-answer-feedback\` with role=status and aria-live=polite). A term-count validator example calls this helper too.

**Consequence:** merely porting the modified factorCheck alone is insufficient: in the old example the helper comes from page-specific authored JavaScript. For a self-contained Ximera feature we must provide a reliable generic helper, decide how it is incorporated into validated page output, and ensure message attachment to the correct answer under nested/multiple-answer conditions. Incorporate it and required validators in **one coherent feature PR** unless the user later changes scope. Don't claim screen-reader validation before testing.

Provenance: https://github.com/xronosuf/ValidatorExamples/blob/main/pageManipulationValidators/advancedFeedback.tex

## Nested PDF styling: review status

\`xronosuf/ximeraLatexDev:jason-problemStyleRevamp/src/pdfstyling.dtx\` contains:
- \`\\newif\\ifdispNest\`; \`\\dispNesttrue\` initial default.
- \`\\newif\\ifdispNestColors\`; \`\\dispNestColorsfalse\` initial default.
- \`\\AtBeginDocument\` guarded by \`\\pdfOnly\`, followed by \`\\ifdispNest\` and tcolorbox wrapping for various theorem and problem environments.

Therefore switches were **implemented in code**, but compile integrity, all selected environment names, nested levels, ability to turn the feature off before document start, and intended defaults are **not yet verified**. Hold as potentially incomplete, and don't bundle with legacy course-specific styling.

## Hierarchical numbering: IMPORTANT COMPATIBILITY GAP

Verified by reading current upstream code and the XimeraProject FirstSteps default-branch CSS:

- Current upstream \`src/options.dtx\` declares \`\\newif\\ifnumberedProblems\`, defaults to false, and registers \`onlineProblemNumbers\` class option.
- Current upstream \`src/problem.dtx\` has its HTML problem-numbering block **commented out**. It defines \`\\problemNumDisp{}\` (empty) and emits an HTML attribute named \`numbered\` on problem elements.
- Current upstream generated class retains a PDF-facing \`\\problemNumber\` mechanism and a default problem-counter scheme, so HTML and PDF behavior should be checked separately.
- \`XimeraProject/ximeraFirstSteps:main/global.css\` uses \`attr(numbered)\` in headings; \`aFirstXourseVariant.css\` also uses \`attr(numbered)\`. One CSS comment explicitly says counting should happen in TeX rather than pure CSS.
- Historical \`xronosuf/ximeraLatexDev:jason-problemStyleRevamp/src/problem.dtx\` partially enables hierarchical number generation and the \`\\ifnumberedProblems\` conditional, **but emits attribute \`probnum\`** rather than \`numbered\`! This name difference is a direct interoperability risk with FirstSteps CSS and any server styling.
- Historical branch's \`\\probNumber\` and \`\\problemNumDisp\` logic should be inspected for nesting, counter reset, and depth beyond five levels; run tests with number option on/off and author's user-level toggle.

**Proposal:** treat as independent numbering feature PR only after resolving the attribute contract. Favor the existing \`numbered\` attribute unless server runtime evidence supports an intentional switch. Preserve unnumbered default; allow author to enable via class option and likely \`\\numberedProblemstrue\` in document preamble (test). Other number formats are a possible extension, not a promise until supported.

Sources:
- https://github.com/XimeraProject/ximeraLatex/blob/master/src/options.dtx
- https://github.com/XimeraProject/ximeraLatex/blob/master/src/problem.dtx
- https://github.com/XimeraProject/ximeraFirstSteps/blob/main/global.css
- https://github.com/xronosuf/ximeraLatexDev/tree/jason-problemStyleRevamp

## Legacy xmsection

Searches against default-branch code of \`XimeraProject/ximeraLatex\` and \`XimeraProject/ximeraFirstSteps\` found no \`xmsection\` reference. Original historical \`xronosuf/ximeraFirstSteps:wim_test-ku-style/.ximera/ximera.cls\` contained \`\\providecommand\\xmsection\\section\` and corresponding subsection alias. User requested dropping if not present in current upstream/firststeps: **DROP**.

## Safety and stage gate

- Source repositories including \`maa4102\`, \`ValidatorExamples\`, XimeraProject upstream and firstSteps remain strictly read-only.
- Work/investigation records are written only to \`xronosuf/ximeraLatexDev:review/ximera-recovery-inventory-20261109\`; the clean upstream baseline remains untouched.
- One standalone PR per new feature, grouping only required supporting changes. Group bug fixes only when technically coherent.
- After classification, obtain user's inventory/PR-boundary review before implementing features on separate review branches; final \`feature/...\` PR heads must exclude all draft/investigation content.

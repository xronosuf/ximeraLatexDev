# Repository and branch screening ledger

Investigation date: 2026-11-09. Review-only, not upstream PR material.

## Methods and limitations

- Enumerated the 30 repos available via the linked `xronosuf` account, plus 2 via `Zengez`; inspected each **default branch** Git tree recursively. All returned complete (`truncated=false`).
- Enumerated branches in all 32 repos. For branches plausibly relevant to XimeraLaTeX, inspected their recursive file trees and, where useful, historical commits and file contents. A few namespaced branches were read by commit SHA to handle slash-containing refs.
- Presence of code is not proof of a successfully deployed feature; absence at the branch tip is not proof there was no older deleted implementation. Git history of known local Ximera package paths has been searched in the highest-priority repos, not every past revision of every file in all 32 projects.
- Search included `.ximera_local/`, `ximera_local-BAK/`, older `.ximera/`, arbitrary local `.cls`/`.4ht`/`.cfg`/`.dtx`, scripts, and course-level `.sty` files. Many hits are old upstream snapshots or course styling, not unique general-purpose features.
- No writes were made to source repositories.

## Priority package sources

| Repo | Branches examined | Package source classification |
| --- | --- | --- |
| `xronosuf/ximeraLatexDev` | master, development, base/upstream-master, 222-a11y, jason-problemStyleRevamp, draft-restructuredDTX | Canonical repo plus historical branches. Nested styling/numbering, possible footnote fix, DTX reorg proofs, docs. Only writable repository. |
| `xronosuf/HaLLMos-embed` | main, master | Master has copied `.ximera_local` with generated-only HaLLMos code and demo .tex. |
| `xronosuf/maa4102` | main | Copied `.ximera_local` with HaLLMos generated-only changes and old incidental edits. |
| `xronosuf/testFiles` | main, tex4npm-testing | Main has copied `.ximera_local` with SagePlot DTX + generated HTML; experimental latest component migration on other branch. |
| `xronosuf/ValidatorExamples` | main | Copied `.ximera_local` with dynamic factorCheck feedback additions; examples. |
| `xronosuf/mac2311` | main, master | Main copied Ximera package; matches common 101-file snapshot. |
| `xronosuf/mac1105` | main, master | Copied Ximera package; matches same 101-file snapshot. |
| `xronosuf/mac1147` | main | Copied Ximera package; matches common snapshot. |
| `xronosuf/mac2312` | main | Copied Ximera package; matches common snapshot. |
| `xronosuf/mac1140` | main, originalContent, work/mac1140-text-rewrite, codex/serve-test-production-buttons-2026-10-25 | Current main has `ximera_local-BAK/`; other branches include `.ximera_local/`; common old snapshot, no new package delta established. |
| `xronosuf/mac2233` | main | No committed Ximera package copy; legacy local `Limits/ximerapdfrevamp.sty` contains tentative PDF styling/QR fallbacks. |
| `xronosuf/ximeraFirstSteps` | main, OLDmain, OLDdevelopment, wim_test-ku-style | Old branches have `.ximera/ximera.cfg` or `.cls`, including `\\xmsection` aliases; attribution unclear. |
| `xronosuf/TestBed` | master | 2020 standalone `Styling/ximera.cls` and largely commented style experiments; no working reusable improvement confirmed. |
| `xronosuf/components` | main, development-container, development-initial | Different `tex4npm` modular Ximera architecture, separate from target legacy class. |
| `xronosuf/server` | production, sageplot, instructor-settings, grade-boundary-passback | No Ximera class/DTX; SagePlot service integration, runtime and documentation on production and branches. |
| `xronosuf/server-1` | master, development, veryold_development, color-theme | No Ximera package copies discovered in inspected branch tips. |

## Other repositories screened (default branches)

| Repo | Finding |
| --- | --- |
| `xronosuf/xake` | Build/server-related files, no Ximera class/DTX copy found. |
| `xronosuf/Problem-Bank` | No Ximera package copy found. |
| `xronosuf/calculus` | No package copy in default or examined alternate branches (development, Nelalakos, two bwramsey branches). |
| `xronosuf/Precalc` | Older course content; no committed package copy in default. |
| `xronosuf/map2302text` | Default and `original` tree: no Ximera package copy identified. |
| `xronosuf/ximeraManuals` | Manuals, no source Ximera package copy. |
| `xronosuf/ximeraDevJason` | README concept proposals for accessibility/intent; no implemented package at default tip. |
| `xronosuf/mac2313` | main and FullBackup/grad-alt-1/grad-alt-2/grad-alt-3: no package copy. |
| `xronosuf/xronosTestRepo` | Content/test repo without package copy. |
| `xronosuf/mac2234` | No package copy found. |
| `xronosuf/MAP2302` | Course-specific and third-party `.sty` files (wrapfig/tasks/etc.), not shown to be Ximera core enhancements. |
| `xronosuf/mac1114` | No package copy found. |
| `xronosuf/sagecell-server` | Server dependency and contract test; no Ximera class files. |
| `xronosuf/mailProcessing` | No Ximera package copy found. |
| `Zengez/mathDef` | No Ximera package source files on default branch. |
| `Zengez/mathThm` | No Ximera package source files on default branch. |

## Notes on completeness

- Branch enumeration is repository-wide; deep historical commit archaeology remains selective, concentrated on actual package snapshots and the branches with changed generated/source files.
- No missing local Codespaces files can be recovered through this GitHub tree approach; those were excluded by the user's scope decision.
- Detecting a historical code artifact does **not** indicate it was authored by the user, accepted by upstream, executed in production, or should be submitted as a PR. Those are separate gates.
- All review records will be excluded from clean feature PR branches.

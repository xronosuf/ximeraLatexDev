# C03 — Dynamic validator feedback: PENDING (deferred by user)

Status: **PENDING / DO NOT IMPLEMENT OR SUBMIT YET.** User decided 2026-11-09 that the original dynamic feedback requires significant polish and packaging, and wants to revisit it separately.

## Resume starting points

- Working example: https://github.com/xronosuf/ValidatorExamples/blob/main/pageManipulationValidators/advancedFeedback.tex
- Modified class validator: https://github.com/xronosuf/ValidatorExamples/blob/main/.ximera_local/src/validators/factorCheck.dtx
- Original validator class snapshots: https://github.com/xronosuf/ValidatorExamples/tree/main/.ximera_local
- Unmodified upstream validator source: https://github.com/XimeraProject/ximeraLatex/blob/master/src/validators/factorCheck.dtx
- Context and earlier audit: reviewNotes/USER_DECISIONS_AND_COMPATIBILITY_20261109.md

## Blocker summary

The demonstration's `displayMessageBelowCurrentAnswer(...)` JavaScript helper is defined inside the authored example, not centrally in XimeraLaTeX. It tracks a current answer/problem and inserts a `.custom-answer-feedback` message; existing implementation can associate responses ambiguously in nested/multiple-answer pages. The modified factorCheck includes guarded helper calls, but in upstream `ximera.dtx` the validatorHook and factorCheck files are presently commented out of the generated class. Simply copying the validator DTX calls cannot deliver a complete feature and would risk unrelated activation.

Future work should design a clean per-answer feedback API, its answer identity and accessibility semantics, packaging/loading, and independently test the lifecycle. Do not rush into this while finishing other upstream PR candidates.

An empty preparatory branch `feature/dynamic-validator-feedback-20261109` exists at upstream baseline only; it contains no changes and does not represent a completed feature.

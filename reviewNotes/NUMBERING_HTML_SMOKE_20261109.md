# Hierarchical HTML numbering — local counter behavior smoke check

Review-only report; no test fixtures added to upstream feature branch.

## Environment

Reconstructed the HTML code from `feature/hierarchical-problem-numbering-20261109` in a local TeX4ht overlay of the installed TeX Live Ximera class (older 2024 package) and set the modern `\ifnumberedProblems` toggle. This was *not* a checkout of the modern `XimeraProject/ximeraLatex` source. The old installed config reports pre-existing `pgfsyspdfmark` and `sagesilent` errors, so make4ht exit status is 1; nevertheless an HTML page is produced.

## Tested scenario and results

Two parent `problem` environments, first parent containing a `question` and an `exercise`, and second parent containing one `question`.

- With `\numberedProblemstrue` set in the preamble, output `<div role=article class=problem-environment ... numbered=...>` contains values: **1, 1.1, 1.2, 2, 2.1** (attribute has intentional leading spaces for separation).
- With the numbering option left false, each `numbered` attribute is empty; numbering is opt-in.
- The first local transcription omitted the end-of-line TeX `%` comments and consequently produced an extra space before dots. Restoring the actual code's comments exactly corrected the output to 1.1/1.2/2.1. Those source comments are functionally required when regenerating `ximera.4ht`; do not strip them casually.
- The recovered port emits the established `numbered` HTML attribute consumed by the existing Ximera CSS; the old historical PR #201 emitted incompatible `probnum`.

## Limitations

The local toolchain is older and has unrelated TeX4ht config errors, so this check is not a clean full modern Ximera build or browser CSS acceptance test. Deep nesting beyond five levels and HTML class option parsing (as distinct from the preamble boolean toggle) were not independently tested. The source `.dtx` and generated `.4ht` HTML region were previously matched.

Branch: https://github.com/xronosuf/ximeraLatexDev/tree/feature/hierarchical-problem-numbering-20261109
Earlier upstream discussion: https://github.com/XimeraProject/ximeraLatex/pull/201

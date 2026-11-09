# Proposed upstream PR — validator authoring/runtime walkthroughs

**Target:** XimeraProject/ximeraLatex:master  
**Head:** xronosuf/ximeraLatexDev:docs/validator-workflow-reference-20261109  
**Status:** Draft; review before submitting.

## Proposed title

Document individual-answer and grouped-validator execution workflows

## Proposed PR body

Add two reference walkthroughs in `usefulReferences/workflowWorkthroughs/`:

- `01-individual-answer-validator.txt` documents the author `\answer[validator=...]` path from source and HTML generation through MathJax, browser evaluation, and answer persistence.
- `02-grouped-validator-environment.txt` documents the separate `\begin{validator}...\end{validator}` workflow and **prominently warns that grouped validators are currently unreliable/not recommended**. It is descriptive documentation, not a fix.

Both documents cite the upstream XimeraLaTeX source and Xronos server runtime source. They distinguish what current code does from potential future designs. No code changes or server changes are included.

### Implementation context

These references were previously added to the author's development fork master but are being proposed separately from a clean upstream base, rather than bundled with any code feature.

### Scope

Documentation only. No regression fixtures are included. The grouped-validator mechanism remains enabled but is NOT claimed reliable or repaired by this PR.

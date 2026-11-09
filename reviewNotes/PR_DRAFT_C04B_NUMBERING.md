# Proposed upstream PR — optional hierarchical HTML problem numbering

**Target:** XimeraProject/ximeraLatex:master  
**Head:** xronosuf/ximeraLatexDev:feature/hierarchical-problem-numbering-20261109  
**Status:** DTX/generated source consistent; local HTML counter smoke verified (modern clean build and browser CSS review outstanding).

## Proposed title

Restore optional hierarchical problem numbering in Ximera HTML output

## Proposed PR body

Restore HTML numbering for nested `problem`, `exercise`, `question`, and `exploration` environments using the existing class option `onlineProblemNumbers` (or the preamble toggle `\numberedProblemstrue`). The default remains unnumbered.

The implementation recovers the nested-counter approach from the earlier `problem style revamp` PR #201 but leaves the PDF problem-numbering implementation unchanged. It emits the established `numbered` HTML attribute instead of the old branch's `probnum`, so existing Ximera server and course CSS using `attr(numbered)` can display the numbers.

This intentionally does not include the PDF tcolorbox styling originally bundled with #201; that is a separate proposal.

### Prior discussion

The earlier proposal is https://github.com/XimeraProject/ximeraLatex/pull/201. Review questions in June 2025 discussed TeX conditional mechanics and CSS. This modernized branch preserves the original feature design while correcting the attribute contract and isolating its scope.

### Verification

Updated `src/problem.dtx` and its generated `ximera.4ht` HTML block were compared for consistency. A local TeX4ht smoke run with the recovered code produced the opt-in sequence **1, 1.1, 1.2, 2, 2.1** for nested problem/question/exercise environments and empty numbered attributes with the option off. The installed older toolchain separately reported unrelated configuration errors, so this does not replace a clean modern build or browser check. Deep nesting and class option parsing still merit external validation. No regression fixtures are included in this PR.

### Unchanged

No class-wide PDF numbering changes or server stylesheet edits. The rest of upstream source remains untouched.

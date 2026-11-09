# Proposed upstream PR — expandable HTML footnote corrections

**Target:** XimeraProject/ximeraLatex:master  
**Head:** xronosuf/ximeraLatexDev:feature/expandable-footnotes-20261109  
**Status:** DTX/generated source consistent and local HTML smoke output checked; modern full Ximera build and browser/accessibility review remain pending.

## Proposed title

Fix expandable footnote disclosure controls and HTML styles

## Proposed PR body

Improve the existing Ximera HTML footnotes in `src/footnotes.dtx` and its regenerated `ximera.4ht` without changing their PDF appearance.

- Make footnote buttons explicit `type="button"` controls.
- Expose `aria-controls`, a per-footnote `aria-label`, and initial `aria-expanded="false"`, updating the expanded state when a footnote opens or closes.
- Avoid a JavaScript exception if a target footnote is missing.
- Remove JavaScript's hard-coded blue color override to respect the configured `fncolor` color.
- Correct malformed inline CSS assignments (`position=relative`, `vertical-align=baseline`, `z-index=9999`, and `overflow=auto`) to valid declarations, and remove the hard-coded blue parent color so the configured footnote text color is respected.

The original inline disclosure function and content structure are preserved. This is an incremental HTML bug/accessibility fix; no new server behavior is required.

### Relationship to other pending changes

The old historical source in `jason-problemStyleRevamp` differs from upstream, but its exact form should not be copied wholesale. Open XimeraProject PR #272 mentions footnote accessibility in its description, but its current changed-file list does not contain `src/footnotes.dtx`. Please check that PR again before merging to avoid conflicts.

### Verification

The new footnote source block was compared against the corresponding generated `ximera.4ht` and matches. A local HTML smoke compilation (using an installed older Ximera class) produced unique button-to-footnote IDs, correct initial `aria-expanded` and `aria-controls`, and output JavaScript that passes a syntax check. That old toolchain also emitted pre-existing unrelated TeX4ht configuration errors, so this is **not** a clean modern Ximera build. Interactive browser checks and assistive-technology review remain to be performed separately.

No regression files are included in this PR.
# Agreed upstream contribution rules — 2026-11-09

## Upstream destination and branch construction
- Build every feature branch from xronosuf/ximeraLatexDev:base/upstream-master, SHA 590672174f137a913598f78e38df831ec23ff5ba, identical to XimeraProject/ximeraLatex:master when checked.
- The development fork master carries only its own standalone documentation contribution. Do NOT use master as the feature-branch parent.
- Upstream PRs ultimately target XimeraProject/ximeraLatex:master. Historical projects and upstream stay read-only until submission is intentionally made.
- One independent PR per feature; review-only notes must never appear on upstream PR branches.

## Hierarchical problem numbering and upstream PR #201
- Recover and modernize the original hierarchical numbering design as a genuine independent feature, while correcting compatibility problems such as the probnum-versus-numbered HTML attribute.
- The maintainer discussion in June 2025 is useful implementation context, not a reason to abandon the design. Bart remains the final upstream arbiter.
- Separate PDF nesting style into its own PR. Keep historical XimeraProject/ximeraLatex PR #201 open until an explicit later closure/supersession decision.

## SagePlot optional/fail-safe integration
- Add SagePlot to the ximeraLatex PR list now. No server-code change belongs in the LaTeX PR. Server work is a separate future project.
- The web output is an inert script marker inside a sage-plot span, with script type text/x-sage-plot, and the authored Sage expression as inert text. LaTeX itself performs no Sage HTTP calls, runtime initialization, or server detection.
- Existing Xronos server support may render the marker. On any other server lacking SagePlot, the marker must safely do nothing and publishing/compilation should not fail.
- Preserve SageTeX PDF command behavior where SageTeX is installed. If sagetex.sty is missing, provide a parseable no-op fallback rather than an undefined-command failure, and document that no plot image appears in this case.
- Respect SageTeX's two optional arguments and mandatory Sage expression. HTML may ignore presentation options, retaining just the passive expression marker.
- Verify absence-of-SageTeX compilation and, separately, the produced HTML semantics before proposing an upstream PR.

## Inventory
Keep candidates C01 HaLLMos, C02 SagePlot, C03 dynamic feedback, C04a nested PDF styling (hold for tests), C04b hierarchical numbering, C06 workflow documentation, C07 expandable footnote fix.
Exclude C05 course-local PDF/QR styles and C08 xmsection aliases, as previously agreed.
Prior review: reviewNotes/UPSTREAM_PR_AND_NUMBERING_AUDIT_20261109.md

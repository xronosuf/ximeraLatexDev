# HaLLMos reconstruction vs MAA4102 — 2026-11-09

**Review-only evidence; not for upstream feature PR branches.**

## Historical published usage and source

- User supplied published activity: https://xronos.clas.ufl.edu/maa4102/Homework1/p05 (page inaccessible to current browser; previous publication is reported by the author, not independently reloaded here).
- Actual activity source: https://github.com/xronosuf/maa4102/blob/main/Homework1/p05.tex, using `\begin{hallmosEnv}[]`, a theorem/identity with inline math and a displayed equation, followed by an explicit caveat about AI-generated proof feedback.
- MAA4102 `xmScripts/config.txt` specifies XIMERA_NAME=maa4102 and XIMERA_URL=https://xronos.clas.ufl.edu/, matching the supplied URL.
- `xronosuf/maa4102:main/.ximera_local/ximera.cls` SHA 03012a29c455f0e6e63564cf7fcbfd5a3d8b060f exactly matches `xronosuf/HaLLMos-embed:master/.ximera_local/ximera.cls`, including the historical `\hallmos` and `hallmosEnv` definitions.
- The two repositories' `ximera.4ht` files have different Git blob hashes but equivalent HaLLMos macro/environment markup. Both inject the hallmos.com/embed.js script followed by `figure.hallmos-embed[data-height=240]` containing a blockquote.

## Proposed modern contribution

- Branch: https://github.com/xronosuf/ximeraLatexDev/tree/feature/hallmos-embed-20261109
- New canonical DTX: `src/interactives/hallmos.dtx`; properly registered in both `ximera.dtx` and `ximera.ins`, with matching generated `ximera.cls` and `ximera.4ht` fragments.
- HTML retains the historical script/figure/blockquote integration. The modern DTX uses TeX4ht paragraph handling for the embedded math; the HTML environment takes the same empty optional argument used by the MAA4102 page.
- PDF deliberately displays the authored statement rather than the old placeholder text; this is a small deliberate improvement from the historical generated class.

## Locally executed checks, with limits

- PDF smoke test: copied the actual MAA4102 theorem statement into a local Ximera document and used the reconstructed DTX's environment definition. The installed TeX Live Ximera class (2024 version) produced a one-page PDF via pdflatex exit 0, with the mathematical expression visibly rendered. This checks grammar/PDF behavior, **not** the full modern feature-branch class build.
- HTML smoke test: overlaid the reconstructed HaLLMos PDF/HTML snippets on the system's installed TeX Live Ximera class and TeX4ht configuration (2024 version). `make4ht` emitted HTML containing the expected `<script src="https://hallmos.com/embed.js">`, `<figure class="hallmos-embed" data-height="240"><blockquote>`, formatted inline math and the displayed identity inside the blockquote. The tool exited 1 because that installed TeX4ht configuration reported `\pgfsyspdfmark already defined` and `sagesilent undefined` errors unrelated to HaLLMos. Do **not** call this a clean HTML build.
- The source and generated class/HTML snippets were separately verified equivalent. No fixtures are present in the feature branch. A fresh modern-upstream clean HTML compile and browser script-runtime check remain outstanding.

## PR review status

Published historical usage is well evidenced and the port produces the expected HTML structure in a smoke test, but full upstream build and remote embed behavior remain unverified. This is suitable for user review of the proposed PR, with those limits explicit.

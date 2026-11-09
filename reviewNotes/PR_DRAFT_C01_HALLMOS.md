# Proposed upstream PR — HaLLMos proof-feedback embed

**Target:** XimeraProject/ximeraLatex:master  
**Head:** xronosuf/ximeraLatexDev:feature/hallmos-embed-20261109  
**Status:** Draft; full build and current remote embed behavior remain to verify.

## Proposed title

Support HaLLMos proof-feedback embeds in Ximera activities

## Proposed PR body

Introduce `\hallmos[metadata]{statement}` and the `hallmosEnv` environment. On the web, these emit a statement-bearing `<figure class="hallmos-embed">` with a `<blockquote>`, and load the external `hallmos.com/embed.js` script. Without that script, the statement remains as readable text.

On PDF output, the statement is printed normally (there is no web proof-feedback widget). The optional metadata argument is parsed but not currently used by the external embed.

### Practical example

- Published activity previously used in an analysis course: https://xronos.clas.ufl.edu/maa4102/Homework1/p05
- Corresponding source: https://github.com/xronosuf/maa4102/blob/main/Homework1/p05.tex

The implementation was reconstructed from the generated class and TeX4ht code in the historical `xronosuf/HaLLMos-embed` repo, into a dedicated `src/interactives/hallmos.dtx`. Both `ximera.dtx` and `ximera.ins` register the new module and the generated class/HTML outputs are included.

### Scope

Stand-alone LaTeX support only; no upstream server changes. The embed depends on a third-party script and this PR does not attempt to implement or validate that remote service.

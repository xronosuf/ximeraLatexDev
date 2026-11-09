# Proposed upstream PR — HaLLMos proof-feedback embed

**Target:** XimeraProject/ximeraLatex:master  
**Head:** xronosuf/ximeraLatexDev:feature/hallmos-embed-20261109  
**Status:** Draft; historical MAA4102 source verified and local TeX4ht/PDF smoke output inspected. Clean modern upstream build and current remote embed behavior remain to verify.

## Proposed title

Support HaLLMos proof-feedback embeds in Ximera activities

## Proposed PR body

Introduce `\hallmos[metadata]{statement}` and the `hallmosEnv` environment. On the web, these emit a statement-bearing `<figure class="hallmos-embed">` with a `<blockquote>`, and load the external `hallmos.com/embed.js` script. Without that script, the statement remains as readable text.

On PDF output, the statement is printed normally (there is no web proof-feedback widget). The optional metadata argument is parsed but not currently used by the external embed.

### Practical example

- Published activity previously used in an analysis course: https://xronos.clas.ufl.edu/maa4102/Homework1/p05
- Corresponding source: https://github.com/xronosuf/maa4102/blob/main/Homework1/p05.tex

The implementation was reconstructed from generated class and TeX4ht code in `xronosuf/HaLLMos-embed` and the actual `xronosuf/maa4102` local Ximera source. The historical classes are byte-identical, and the HaLLMos HTML structures agree. The feature is now maintained in `src/interactives/hallmos.dtx`, registered through `ximera.dtx` and `ximera.ins`, with corresponding generated class/HTML output.

### Verification

The MAA4102 problem source was compiled in a local PDF smoke test with the recovered environment definition, producing the expected mathematical statement. A local TeX4ht smoke run emitted a HaLLMos figure and displayed mathematics matching the historical embed structure, but the older installed Ximera/TeX4ht toolchain also reported unrelated configuration errors. These smoke checks are **not** a successful clean build of the current upstream XimeraLaTeX class. Generated class/HTML snippets were matched against the DTX source.

### Scope

Stand-alone LaTeX support only; no upstream server changes. The embed depends on a third-party script and this PR does not attempt to implement or validate that remote service.

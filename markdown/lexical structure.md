Ivy's lexical structure has the following categories of token:

  - **Keywords** listed on their [[keywords|own page]].
  - **Comments** which begin with `#` and extend to the end of the line.
  - **Punctuation** elements drawn from the following list: `~`, `,`, `+`, `*`, `/`, `-`, `<`, `<=`, `>`, `>=`, `*>`, `(`, `)`, `|`, `&`, `=`, `~=`, `;`, `:=`, `.`, `{`, `}`, `->`,  `<->`, `:`, `..`, `...`, `$`, `^`
  - **Symbols** which begin with a lower-case letter, underscore or digit and continue with any mix of letters, digits and underscores. These typically name types, modules, objects, parameters, local variables, and similar declarations. These also include numeral-literals and boolean-literals in expressions. For example: `a`, `_v_`, `10`, `foo123`, `lowerUPPER`, `false`, `this`
  - **Variables** which begin with upper-case letter and continue with any mix of letters, digits and underscores. These denote "logical variables" bound by a [[universal quantifier]] or [[existential quantifier]] in an [[expression]]. For example: `A`, `VAR_12`, `Thing`
  - **Labels** which are any letters, digits or underscores contained in square brackets. These typically denote logical formulas and are declared in-line with many [[logical judgment|logical judgments]]. For example: `[premise]`, `[f0]`, `[NO_WAY]`
  - **Native Quotations** which are any text between a matching pair of triple angle brackets (`<<<` and `>>>`)

Ivy is whitespace-insensitive, beyond the need to separate tokens.


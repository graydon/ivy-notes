[[keywords|Keyword]]: `macro`

Declares a special sort of action that only has meaning to a Dafny code-generator, unused in general. Dafny apparently has to differentiate call-by-value and call-by-reference actions.

Not to be mistaken for a [[macro definition]] which is introduced with the `definition` keyword.

Almost all uses of the term "macro" in Ivy's documentation and user interface refer to macro definitions, **not** macro actions. The `macro` keyword should almost never be used.
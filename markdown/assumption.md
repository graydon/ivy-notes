## As a keyword

[[keywords|Keyword]]: `assume`

Introduces a logical assumption into a context.

Exists as both a [[tactic]] and a [[statement]].

## As a concept

"Assumption" as a general concept also refers to a logical assumption introduced by any of:

  - The `assume` keyword itself, which adds a general free-standing assumption in an [[action]] not associated with the action's [[visibility qualifiers]] or [[assume-guarantee reasoning]] between actions.
  - A [[precondition]] (keyword `require`) when considered from the perspective of verifying the [[action]] containing the `require` statement.
  - A [[postcondition]] (keyword `ensure`) when considered from the perspective of verifying an [[action]] that **calls** the action containing the `ensure` statement.
  - An [[invariant]] (keyword `invariant`) which also introduces an [[guarantee]] (as invariants are **inductive**, they both assume and guarantee).

Assumptions and [[guarantee|guarantees]] together support [[assume-guarantee reasoning]], which is a basis of [[isolate|modular verification]] in Ivy.

Assumptions occur [[positive and negative occurrences|positively]] in [[verification condition|verification conditions]].
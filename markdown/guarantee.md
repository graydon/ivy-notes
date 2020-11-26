A guarantee is introduced by any of:
  - An [[assertion]] (keyword `assert`) which is a general free-standing guarantee in an [[action]] not associated with the action's [[visibility qualifiers]] or [[assume-guarantee reasoning]] between actions.
  - A [[postcondition]] (keyword `ensure`) when considered from the perspective of verifying the [[action]] containing the `ensure` statement.
  - A [[precondition]] (keyword `require`) when considered from the perspective of verifying an [[action]] that **calls** the action containing the `require` statement.
  - An [[invariant]] (keyword `invariant`) which also introduces an [[assumption]] (as invariants are **inductive**, they both assume and guarantee).

Guarantees together with [[assumption|assumptions]] support [[assume-guarantee reasoning]], which is a basis of [[isolate|modular verification]] in IVy.

Guarantees occur [[positive and negative occurrences|negatively]] in [[verification condition|verification conditions]].
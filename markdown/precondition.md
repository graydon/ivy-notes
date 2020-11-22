[[keywords|Keyword]]: `require`

A precondition introduces an [[assumption]] in the action `A` it occurs within, and a [[guarantee]] in any action that calls `A`.

This includes calls from the [[environment]]: the environment is presumed to have fulfilled any guarantee imposed on it by a precondition in an action it calls.

A precondition is a statement with strictly logical content. It does not modify state variables.

The exact relationship between callee preconditions and caller [[verification condition|verification conditions]] is discussed in the [[weakest liberal precondition]] page, and the page on [[assume-guarantee reasoning]].

See also [[postcondition|postconditions]], the symmetric concept.
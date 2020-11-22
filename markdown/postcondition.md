[[keywords|Keyword]]: `ensure`

A postcondition introduces a [[guarantee]] in the action `A` it occurs within, and an [[assumption]] in any action that calls `A`, in the state immediately following the call.

A postcondition is a statement with strictly logical content. It does not modify state variables.

The exact relationship between callee postconditions and caller [[verification condition|verification conditions]] is discussed in the [[weakest liberal precondition]] page, and the page on [[assume-guarantee reasoning]].

See also [[precondition|preconditions]], the symmetric concept.
[[keywords|Keyword]]: `invariant`

## In a [[module]]
An invariant [[declaration]] is a [[primitive judgment]] associated with a specific [[isolate]], [[object]] or [[module]] (or at top-level, the program's global module). Its is [[verification condition|verified]] by checking that it is an **inductive** invariant. This involves two steps:

  - Initiation: checking whether the isolate's [[initializer]] establishes the invariant.
  - Consecution: checking whether every [[export|exported]] [[action]] within the isolate, individually, preserves the invariant.

When verifying an invariant's consecution for a given action, the invariant is [[assumption|assumed]] before the action and must be [[guarantee|guaranteed]] by the action.

## In a [[loop]]

A [[loop]] invariant is an annotation on a loop body that provides both an [[assumption]] and [[guarantee]] about each iteration of the loop -- effectively an [[assertion]] on the state variables at both the beginning and end of the loop body.
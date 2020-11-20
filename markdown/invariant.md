[[keywords|Keyword]]: `invariant`

An invariant is a [[primitive judgment]] associated with a specific [[isolate]] (or at top-level, the program's global module). Its is [[verification condition|verified]] by checking that it is an **inductive** invariant. This involves two steps:

  - Initiation: checking whether the isolate's [[initializer]] establishes the invariant.
  - Consecution: checking whether every [[export|exported]] [[action]] within the isolate, individually, preserves the invariant.


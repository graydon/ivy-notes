[[keywords|Keyword]]: `init`

Every object runs a single [[export|exported]] [[action]] at the beginning of its existence called its **initializer**.

This action is declared by the special block `after init { ... }`, which is to say it appears syntactically as a [[mixin]] for a special reserved action named `init`

Initializers typically exist to support verification of inductive [[invariant|invariants]], which in turn support verifying the [[precondition|preconditions]] of other [[action|actions]].
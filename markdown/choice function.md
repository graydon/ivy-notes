[[keywords|Keyword]]: `some`, `in`

A **choice function** is a special form of [[definition]] that introduces a different [[axiom]] than usual: one with an implication operator (`->`) guarded by an [[existential quantifier]].

## Example:

Suppose we want to define a function `finv` that is the inverse of function `f`. We can write the definition like this:

```
definition finv(X) = some Y. f(Y) = X in Y
```

This special form of definition says that `finv(X)` is `Y` for *some* `Y` such that `f(Y) = X`. If there is no such `Y`, `finv(X)` is left undefined. The corresponding axiom is:

```
axiom forall X. ((exists Y. f(Y) = X) -> f(finv(X)) = X)
```

With this definition, `finv` is a function, but it isn't fully specified.  If a given element `Y` has two inverses, `finv` will yield one of them. This isn't a [[nondeterministic choice]], however. Since `f` is a function, it will always yield the *same* inverse of any given value `Y`.

If we want to specify the value of `finv` in case there is no inverse, we can write the definition like this:

```
definition finv(X) = some Y. f(Y) = X in Y else 0
```

The `else` part gives us this additional axiom:

```
axiom forall X. ((~exists Y. f(Y) = X) -> finv(X) = 0)
```

Notice that this axiom contains a quantifier alternation. If this is a problem, we could use a macro instead:

```
definition finv(x) = some Y. f(Y) = x in Y else 0
```

The axiom we get is

```
axiom (~exists Y. f(Y) = x) -> finv(x) = 0)
```

which is alternation-free.

[[keywords|Keyword]]: `fresh`

A freshness qualifier may be applied to a [[schema|compound judgment]] premise (such as a `function`, `relation`, `individual`).

By marking a premise as `fresh`, any [[judgment application|application]] of the judgment has to supply a [[fresh name]] for the premise.

## Example:

```
schema elimE = {
    type t
    relation p(X:t)
    property exists X. p(X)
    fresh individual n:t
    #----------------------
    property p(n)
}

type t
relation succ(X:t,Y:t)
axiom exists Y. succ(X,Y)
function next(X:t):t

property succ(X,next(X))
proof {
    apply elimE with
        n = next(X),
        p(Y) = succ(X,Y)
}
```

In this example, the `elimE` [[schema]] is defined as requiring a [[fresh name]] for the premise `n`. When the schema is [[judgment application|applied]] in the proof, `n` is bound to `next(X)`, which is fresh since no [[definition|definitions]] or [[property|properties]] have mentioned it yet.
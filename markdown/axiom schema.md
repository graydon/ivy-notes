One of the [[schema|compound judgments]].

Like [[primitive axiom|primitive axioms]], admitted without [[proof]] to the current context.

## Example:

```
axiom [congruence] {
    type d
    type r
    function f(X:d) : r
    #--------------------------
    property X = Y -> f(X) = f(Y)
}
```

The keyword `axiom` tells IVy that this schema should be taken as valid without proof
[[keywords|Keywords]]: `axiom`, `schema`

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

The keyword `axiom` tells Ivy that this schema should be taken as valid without proof. The braces indicate that it is a [[schema]] and not a [[primitive axiom]].

The same axiom schema can be written as

```
schema congruence = {
    type d
    type r
    function f(X:d) : r
    #--------------------------
    property X = Y -> f(X) = f(Y)
}
```

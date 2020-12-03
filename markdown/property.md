[[keywords|Keyword]]: `property`

One of the [[primitive judgment|primitive judgments]].

A property is a [[proposition]] that can be admitted as true only if it follows logically from judgments in the current context. A property requires a [[proof]] -- this differentiates it from a [[primitive axiom]].

If a proof is not supplied, IVy applies its default proof [[tactic]].  This calls the automated prover Z3 to attempt to prove the property from the previously admitted judgments in the current context.

The default tactic works by generating a [[verification condition]] to be checked by Z3. This is a [[formula]] whose validity implies that the property is true in the current context.

## Example:
```
property [myprop] r(n,X) -> r(X,n)
```

In this case, the verification condition is:

```
(forall X,Y. r(X,Y) -> r(Y,X)) -> (r(n,X) -> r(X,n))
```

That is, it states that [[axiom]] `symmetry_r` implies property `myprop`.  IVy checks that this formula is within the FAU [[logical fragment]] that Z3 can decide, then passes the formula to Z3. If Z3 finds that the formula is valid, the property is admitted.
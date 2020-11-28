[[keywords|Keyword]]: `named`

A named existential binds a [[skolemization|skolem function]] to a [[fresh name]].

## Example:

If we can prove that something exists, we can give it a name.  For example, suppose that we can prove that, for every `X`, there exists a `Y` such that `succ(X,Y)`. Then there exists a (Skolem) function that, given an `X`, produces such a `Y`. We can define such a function called `next` in the following way:

```
relation succ(X:t,Y:t)
axiom exists Y. succ(X,Y)

property exists Y. succ(X,Y) named next(X)
```

Provided we can prove the property, and that `next` is fresh, we can infer that, for all `X`, `succ(X,next(X))` holds. Defining a function in this way, (that is, as a Skolem function) can be quite useful in constructing a proof.  However, since proofs in Ivy are not generally constructive, we have no way to *compute* the function `next`, so we can't use it in extracted code.

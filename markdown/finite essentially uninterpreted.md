"Finite essentially uninterpreted" (FEU) is a [[logical fragment]] of [[first-order logic]] that extends [[EPR]] with

  - The concept of [[relevant vocabulary|relevant vocabularies]], which are themselves an extension of the concept of [[stratification]]. Specifically: an FEU formula must have finite (that is, acyclic) relevant vocabularies.
  - Limited-use [[theory instantiation|interpreted types]]. Specifically: in an FEU formula, [[universal quantifier|universally quantified]] [[logical variable|logical variables]] must appear only as arguments of [[uninterpreted function|uninterpreted functions]]. Put another way: interpreted types and universal quantifiers must not be combined.

## Example:

As an example, this set of formulas is in the FEU fragment,  where `f`, `g` and `h` are functions over integers:

```
g(X, Y) = 0 | h(Y) = 0
g(f(X), b) + 1 <= f(X)
h(b) = 1
f(a) = 0
```

The equality predicate on integers is also considered interpreted here. Since `X` and `Y` occur only under uninterpreted functions `f`, `g`, `h` (and as the reader can confirm, the relevant vocabularies are finite) the conjunction of these formulas is in FEU.

Ivy checks this condition and will report cases of variables occurring under interpreted operators, with the exception of cases allowed by the slightly-larger fragment [[FAU]].
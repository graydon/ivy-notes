[[keywords|Keyword]]: `forall`

The universal quantifier (written as `forall` in IVy, but more conventionally as `∀` in formal logic) asserts that "for all" elements of some [[sort]], the remaining quantified [[formula]] is true.

The universally-quantified [[logical variable]] follows the keyword and must be a [[lexical structure|lexical variable]] (i.e. must begin with an upper-case letter.)

The variables bound by the quantifier are separated from the formula they quantify over by a period (`.`) though in IVy this can also be accomplished using parentheses (see example below).

Universal quantifiers may be eliminated from a formula by [[explicit quantifier instantiation]].

See also [wikipedia's page](https://en.wikipedia.org/wiki/Universal_quantification) on universal quantifiers.

## Example

```
forall X:t . f(X)
```

This formula says "for all `X` of type `t`, `f(X)` is true".

It may also be written with parentheses, if the type in question contains a period (eg. if it represents a dot-notation path to access a member of a [[module]]). In other words, this is the same expression:

```
forall (X:t) f(X)
```
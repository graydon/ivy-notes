[[keywords|Keyword]]: `exists`

The existential quantifier (written as `exists` in Ivy, but more conventionally as `∃` in formal logic) asserts that "there exists" some elemement of a given [[sort]] for which the remaining quantified [[formula]] is true.

The existentially-quantified [[logical variable]] follows the keyword and must be a [[lexical structure|lexical variable]] (i.e. must begin with an upper-case letter.)

The variables bound by the quantifier are separated from the formula they quantify over by a period (`.`) though in Ivy this can also be accomplished using parentheses (see example below).

Existential quantifiers may be eliminated from a formula by [[skolemization]] and/or [[explicit quantifier instantiation]].

See also [wikipedia's page](https://en.wikipedia.org/wiki/Existential_quantification) on existential quantifiers.

## Example

```
exists X:t . f(X)
```

This formula says "there exists some `X` of type `t` such that `f(X)` is true".

It may also be written with parentheses, if the type in question contains a period (eg. if it represents a dot-notation path to access a member of a [[module]]). In other words, this is the same expression:

```
exists (X:t) f(X)
```
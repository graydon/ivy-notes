A logical [[variable]] is a [[term]] introduced by a [[universal quantifier|universal]] or [[existential quantifier|existential]] quantifier.

Logical variables in Ivy are always written as [[lexical structure|lexical variables]], that is as tokens beginning with a capital letter (eg. `X`, `Foo`, `P123`).

A [[free variable|free logical variable]] (occurring outside of any quantifier) is implicitly universally quantified. Some forms of declarations and statements make use of this as an abbreviation.

A logical variable should not be confused with a [[state variable]]:

  - State variables are [[ground term|ground terms]] themselves, that represent the possible targets of [[assignment]] [[statement|statements]], as [[action|actions]] execute.
  - Logical variables are placeholders for [[ground term|ground terms]] in a [[first-order logic]] formula.

## Example:

Suppose we have the formulas

```
type t
type u
var s:t
function f(X:t): u
```

The [[type]] `t` is a ground term, as is the type `u`.

The [[declared variable]] `s` is a [[state variable]] and is also a ground term of type `t`.

The [[function]] `f` is an [[uninterpreted function]] without a definition, so is also a [[state variable]]. The function symbol `f` on its own is a ground term, but a quantified formula like `forall X. f(X)` is not a ground term because it contains a logical variable `X`.

An application of `f` to a ground term, like `f(s)`, is a ground term of type `u`.

In a formula with an explicit logical variable such as:
```
forall V:t. V = s
```

We have a new logical variable `V` which may range over ground terms of type `t`, such as `s` or `f(s)`. That is, the formula may be instantiated as `s = s` or `f(s) = s`, each of which is a ground term of type `bool`.

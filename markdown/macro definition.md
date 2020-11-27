A **macro definition** is a [[definition]] that is only "unfolded" when it is used. 

Not to be confused with a [[macro action]] declared with the `macro` keyword.

## Example:

Suppose we want to define a predicate `rng` that is true of all the elements in range of function `f`. We could write it like this:

```
definition rng(X) = exists Y. f(Y) = X
```

The corresponding axiom might be problematic, however. Writing it out with explicit quantifiers, we have:

```
axiom forall X. (rng(X) <-> exists Y. f(Y) = X)
```

This formula has an [[quantifier alternation|alternation of quantifiers]] that might result in [[verification condition|verification conditions]] that IVy can't [[logical fragment|decide]]

Suppose though, that we only need to know the truth value of `rng` for some specific arguments. We can instead write the definition like this:

```
definition rng(x:t) = exists Y. f(Y) = x
```

Notice that the argument of `rng` is a constant `x`, not a place-holder `X`. This definition acts like a macro (or an [[axiom schema]]) that can be instantiated for specific values of `x`. So, for example, if we have an [[assertion]] to prove like this:

```
ensure rng(y)
```

IVy will instantiate the definition like this:

```
axiom rng(y) <-> exists Y. f(Y) = y
```

In fact, all instances of the macro will be alternation-free, since IVy guarantees to instantiate the macro using only [[ground term|ground terms]] for the constant arguments.  A macro can have both [[variable|variables]] and constants as arguments. For example, consider this definition:

```
definition g(x,Y) = x < Y & exists Z. Z < x
```

Given a term `g(f(a),b)`, Ivy will instantiate this macro as:

```
axiom g(f(a),Y) = f(a) < Y & exists Z. Z < f(a)
```
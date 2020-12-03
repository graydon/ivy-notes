Skolemization is the process of replacing existentially-quantified _variables_ with _functions_ of existing univerally-quantified variables, producing an equi-satisfiable formula with prenex consisting strictly of universal quantifiers.

As free (unbound) function symbols in first-order formulae are, for the sake of satisfiability,  _implicitly_ existentially quantified, this essentially "hoists existential quantifiers" out of the inside of a formula, moving them to the outermost level where they quantify over [[uninterpreted function|uninterpreted functions]] and can be ignored: any satisfiability query for `f:T->U` is the same as a query for `exists f. f:T->U`.

See also [wikipedia's page](https://en.wikipedia.org/wiki/Skolem_normal_form)

Skolemization is done automatically in many contexts in Ivy, but is also available as a [[tactic]] called `tactic skolemize` which can be manually applied during a [[proof]]. This tactic Skolemizes all the **premises** (but not the conclusion) of a proof goal. This is typically done before [[explicit quantifier instantiation]] in order to [[recovering decidability|recover decidability]].

## Examples:


### Example: basic Skolemization

Initial formula `forall X:t . exists Y:u . f(x,y)`
Skolemizes as: `forall X:t . f(x,s(x))` (introducing the Skolem function `s:t->u`)


### Example: Skolemization in support of quantifier instantiation

Suppose we are attempting to prove this [[theorem]]:

```
theorem [sk1] {
    type t
    individual a : t
    relation r(X:t,Y:t)
    property [prem] forall X. exists Y. r(X,Y)
    #---
    property exists Z. r(a,Z)
}
```

This theorem says "if we have some individual `a:t` and some relation `r(X,Y)`, and we have established as a premise that `forall X. exists Y. r(X,Y)`, then we can conclude `exists X. r(a, Z)`".

In other words: if `r` models the `(input, output)` pairs of an injective function, then that function is defined at input `a`.

Applying `tactic skolemize` implicitly transforms this to a theorem that states **exactly that**:

```
theorem [sk1] {
    function _Y(V0:t) : t
    type t
    individual a : t
    relation r(V0:t,V1:t)
    property [prem] forall X. r(X,_Y(X))
    property exists Z. r(a,Z)
}
```

Here the Skolem function `_Y:(V0:t) : t` has been introduced and the premise property `[prem]` rewritten to say that `r` models the function `_Y` injectively: `forall X. r(X, _Y(X))`

When `tactic skolemize` is done in this example, the premises have only universal quantifiers, and the conclusion has only existential quantifiers. Now, we can finish the proof by [[explicit quantifier instantiation|instantiating]] the universal `X` in `prem` with `a`, giving `r(a,_Y(a))`, and then [[explicit quantifier instantiation|instantiating]] the existentially quantified `Z` in the conclusion with `_Y(a)` , which also gives `r(a,_Y(a))`.

One way to describe the instantiation of the conclusion is that we have provided the term `_Y(a)` as a **witness** for the existential quantifier.

The completed proof is therefore:

```
proof {
    tactic skolemize
    instantiate prem with X=a
    instantiate with Z = _Y(a)
}
```

All of this wasn’t really necessary, since the original theorem is in the [[logical fragment|decidable fragment]]. However, the approach of Skolemization followed by selective instantiation of quantifiers can be a quick way to reduce a proof problem to a decidable one. First, we try to get Z3 to discharge the proof goal automatically. If Ivy complains about a quantified variable, we eliminate that variable by Skolemization and instantiation. If we guess the wrong instantiation, Ivy will give us a counterexample.


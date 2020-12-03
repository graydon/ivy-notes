
EPR ("effectively propositional") is a [[logical fragment|fragment]] of [[first-order logic]] restricted to relational first-order formulas (i.e., formulas over a vocabulary that contains constant symbols and relation symbols but **no function symbols**) with a [[quantifier alternation|quantifier prefix]] ``∃∗∀∗`` in [[prenex normal form]].

Satisfiability of EPR formulas is decidable. It is NEXPTIME-complete but in practice SMT solvers can decide many practical instances in a reasonable amount of time.

Moreover, formulas in this fragment enjoy the [finite model property](https://en.wikipedia.org/wiki/Finite_model_property), meaning that a satisfiable formula is guaranteed to have a finite model. The size of this model is bounded by the total number of existential quantifiers and constants in the formula. The reason for this is that given an `∃∗∀∗`-formula, we can obtain an equi-satisfiable quantifier-free formula by [[skolemization]], i.e., replacing the existentially quantified variables by constants, and then instantiating the universal quantifiers for all constants.

See also the [wikipedia page](https://en.wikipedia.org/wiki/Bernays%E2%80%93Sch%C3%B6nfinkel_class).

## Example

Suppose we take the [[axiom|axioms]] of a partial order as [[assumption|assumptions]]:

```
forall X,Y,Z. X < Y & Y < Z -> X < Z
forall X,Y. ~(X < Y & Y < Z)
```

Notice these are in EPR. The [[verification condition|VC]] for the following program is also in EPR:

```
require forall X,Y. r(X,Y) -> X > Y;
if r(x,y) & r(y,z) {
    r(x,z) := true
};
ensure forall X,Y. r(X,Y) -> X > Y;
```

To see this, let’s expand it out to the conjunction of the following formulas in prenex normal form:

```
forall X,Y. r(X,Y) -> X > Y
r(x,y) & r(y,z) -> (r'(x,z) & forall X,Y. X ~= x | Y ~= y | r'(X,Y) = r(X,Y))
~(r(x,y) & r(y,z)) -> r'(X,Y) = r(X,Y))
exists X,Y. ~(r'(X,Y) -> X > Y)
```

The first of these formulas says that the [[precondition]] holds.

The second says that if we take the “`if`” branch, then `r` is updated so that `r(x,z)` holds and otherwise it remains unchanged.

The third says that if we take the “`else`” branch, `r` is unchanged.

The last says that, finally, the [[guarantee]] is `false`.

Notice that a fresh symbol `r’` was introduced. Technically, this symbol was introduced to allow us to move a universal quantifier on `r` to prenex position without "capturing" other occurrences of `r`. However, we can think of it as just the “next” value of `r`, after the assignment (that is: treating the program as a [[transition relation]] between `r` and `r'`)

We can see that the [[precondition]] and the constraint defining the semantics of assignment both occur [[positive and negative occurrences|positively]]. These formulas are in EPR, and so the corresponding conjuncts of the negated VC also are.

The [[postcondition|guarantee formula]] occurs negatively as `exists X,Y. ~(r'(X,Y) -> X > Y)`. That is, when we see `~forall X. p(X)`, we convert it to the equivalent `exists X. ~p(X)` in prenex form. This formula is also in EPR. In fact, Ivy will convert it to `~(r'(a,b) -> a > b)`, where `a` and `b` are fresh constant symbols.

In general, if we don’t use function symbols, and if all of our assumptions and guarantees are `forall` formulas (so-called "A"-formulas), then the negated VC will be in EPR.


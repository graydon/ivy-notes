A **verification condition** is a [[formula]] that, if valid, implies the correctness of an [[action]], [[logical judgment]], or logical [[statement]].

Ivy's default [[tactic]] for a [[proof]] automatically generates a verification condition for the context requiring the proof. If the verification condition does not fall within the FAU [[logical fragment]], the user will need to participate in [[recovering decidability|constructing a proof or changing the context]].

The same default verification condition generation tactic is involved in proving a free-standing [[logical judgment]] or an [[assertion]], [[precondition]] or [[postcondition]] statement inside an action. Only the input to the tactic varies depending on context.

This tactic can also be invoked explicitly within a manual proof with `tactic vcgen`.

Anywhere a proof is required but not provided, it is equivalent to there being a proof consisting of `tactic vcgen`.

Verification conditions in Ivy are generated from the calculus of [[weakest liberal precondition|weakest liberal preconditions]].

For example, the following program:

```
type t
interpret t -> nat

action decr(x:t) returns (y:t) = {
    require x > 0;
    y := x - 1;
    ensure y < x;
}

export decr
```

has the following VC:

```
x > 0 -> x - 1 < x
```

See the page on [[weakest liberal precondition]] for details on exactly how it is calculated.

## Checking VCs with SMT solvers

A typical [SMT solver](https://en.wikipedia.org/wiki/Satisfiability_modulo_theories) solver (including Z3, the one Ivy uses) can determine definitely whether this [[formula]] is satisfiable, since it is expressed in the form of affine constraints over the natural numbers without quantifiers. Solving constraints of this kind is an NP-complete problem. This means that all known solution algorithms use exponential time in the worst case, but in practice we can almost always solve problems that have a moderate number of variables.

More generally, a typical SMT solver can handle a theory called QFLIA, which stands for "quantifier-free linear integer arithmetic" and allows us to form arbitrary combinations of affine constraints with "and", "or" and "not". We can easily reduce formulas with natural-number variables to formulas using only integer variables, so the solver doesn't need a special theory for natural numbers.

Technically, the way this is done is by *negating* the formula, then passing it to the solver to determine if it is *satisfiable*. In this case, the negated verification condition is:

```
~(x > 0 -> x - 1 < x)
```

which is logically equivalent to:

```
x > 0 & x - 1 >= x
```

We can easily see that this is unsatisfiable, in the sense that there is no natural number `x` that makes it true.

If the negated verification condition has a solution, it means that the verification condition is not valid, so something is wrong with our proof. Suppose, for example, we change the precondition of action `decr` from `x > 0` to `x < 42`. The negated verification condition becomes:

```
x < 42 & x - 1 >= x
```

## Models and counter-models

In Ivy's natural number theory, we have `0 - 1 = 0`. That means that the above formula is actually true for `x = 0`. The assignment `x = 0` is called a *model* of the formula, that is, it describes a possible situation in which the formula is true. That means the assignment `x = 0` is also a *counter-model* for the verification condition: it shows
why the proof doesn't work.

Counter-models are extremely important from the point of view of transparency.  That is, if our proof fails, we need a clear explanation of the failure so we can correct the system or its specification.

## Positive and negative occurrences of formulas in VCs

Verification conditions for even moderately complex programs are big messy formulas that are hard to read. Fortunately, from the point of view of decidability, we need not be concerned with the exact form of the VC. Rather, for each formula occurring in the program or its specifications, we will be concerned with whether the formula occurs [[positive and negative occurrences|positively in the VC, or negatively]] or both. 


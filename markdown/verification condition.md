A **verification condition** is a [[formula]] that, if valid, implies the correctness of a [[program]] (or [[action]], [[logical judgment]], or logical [[statement]]).

IVy's default [[tactic]] for a [[proof]] automatically generates a verification condition for the context requiring the proof. If the verification condition does not fall within the FAU [[logical fragment]], the user will need to participate in constructing a proof.

The same default verification condition generation tactic is involved in proving a free-standing [[logical judgment]] or an [[assertion]], [[precondition]] or [[postcondition]] statement inside an action. Only the input to the tactic varies depending on context.

This tactic can also be invoked explicitly within a manual proof with `tactic vcgen`.

Anywhere a proof is required but not provided, it is equivalent to there being a proof consisting of `tactic vcgen`.

Verification conditions in IVy are generated from the calculus of [[weakest liberal precondition|weakest liberal preconditions]].

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

A typical [SMT solver](https://en.wikipedia.org/wiki/Satisfiability_modulo_theories) solver (including Z3, the one IVy uses) can determine definitely whether this [[formula]] is satisfiable, since it is expressed in the form of affine constraints over the natural numbers without quantifiers. Solving constraints of this kind is an NP-complete problem. This means that all known solution algorithms use exponential time in the worst case, but in practice we can almost always solve problems that have a moderate number of variables.

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

Verification conditions for even moderately complex programs are big messy formulas that are hard to read. Fortunately, from the point of view of decidability, we need not be concerned with the exact form of the VC. Rather, for each formula occurring in the program or its specifications, we will be concerned with whether the formula occurs *positively* in the VC, or *negatively* or both. 

A positive occurrence is one under an even number of negations, while a negative occurrence is under an odd number. For example, in the following formula:

```
~(~P | Q)
```

`P` occurs positively and `Q` occurs negatively. In the formula `P -> Q`, `P` occurs negatively and `Q` positively, since this is equivalent to `~P | Q`. In the formula `P <-> Q`, `P` and `Q` occur *both* positively and negatively, since this is equivalent to `(P -> Q) & (Q -> P)`.

In the negated verification conditions, generally speaking, an assumption occurs positively, while a guarantee occurs negatively. Assignments in the code behave like assumptions.  To see this, we can rewrite the semantics of assignment using a quantifier, like this:

```
wlp(y := e, R) = R[e/y]
               = forall y. y = e -> R
```

Using this method, and converting to [prenex normal form](https://en.wikipedia.org/wiki/Prenex_normal_form), the negated VC for our example becomes a conjunction of the following three formulas:

```
x > 0
y = x - 1
~(x < y)
```

We can see that the assumption `x > 0` occurs positively, the assignment `y = x - 1` occurs positively as an equation, and the guarantee `x < y` occurs negatively. 

On the other hand, as noted above, the VC's for a program invariant *I* have this form: `I -> wlp(a,I)`. This means that the invariant *I* occurs both positively and negatively (or put another way, it is both an assumption and a guarantee). 

Understanding which formulas occur positively and negatively in the negated VC will be important in understanding why the VC is or is not in the [[logical fragment|decidable fragment]].

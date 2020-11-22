A **positive** occurrence of a sub-formula within a [[formula]] is one under an even number of negations, while a **negative** occurrence is under an odd number.

## Example:
For example, in the following formula:

```
~(~P | Q)
```

`P` occurs positively and `Q` occurs negatively. In the formula `P -> Q`, `P` occurs negatively and `Q` positively, since this is equivalent to `~P | Q`. In the formula `P <-> Q`, `P` and `Q` occur *both* positively and negatively, since this is equivalent to `(P -> Q) & (Q -> P)`.

In the negated [[verification condition|verification conditions]], generally speaking, an [[assumption]] occurs positively, while a [[guarantee]] occurs negatively.

[[assignment|Assignments]] in the code behave like assumptions.  To see this, we can rewrite the semantics of [[assignment]] using a [[quantifier]], like this:

```
wlp(y := e, R) = R[e/y]
               = forall y. y = e -> R
```

Using this method, and converting to [[prenex normal form]], the negated VC for our example becomes a conjunction of the following three formulas:

```
x > 0
y = x - 1
~(x < y)
```

We can see that the assumption `x > 0` occurs positively, the assignment `y = x - 1` occurs positively as an equation, and the guarantee `x < y` occurs negatively. 

On the other hand, as noted above, the VC's for a program [[invariant]] `I` have this form: `I -> wlp(a,I)`. This means that the invariant `I` occurs both positively and negatively (or put another way, it is both an assumption and a guarantee). 

Understanding which formulas occur positively and negatively in the negated VC will be important in understanding why the VC is or is not in the [[logical fragment|decidable fragment]].

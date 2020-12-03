The **weakest liberal precondition** (WLP) calculus is a standard form of Dijkstra's [predicate-transformer semantics](https://en.wikipedia.org/wiki/Predicate_transformer_semantics) for calculating [[verification condition|verification conditions]] of imperative programs.

If `S` is a program [[statement]] and `R` is a some condition on the program state, `wlp(S,R)` is the weakest condition `P` such that, if `P` holds before the execution of `S` and if `S` terminates, then `R` holds after the execution of `S`.

The `wlp` function is in other words a "predicate transformer" that works backwards from the end of the program towards its beginning, transforming the predicate `R` (the [[verification condition]] that serves as precondition for the "rest of the program" after `S`) into a predicate `wlp(S,R)` that's a verification condition covering `S` as well.

As an example, consider the following Ivy code:

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

The verification condition for this program can be written as:

```
x > 0 -> wlp(y := x - 1, y < x)
```

That is, the [[precondition]] `x > 0` has to imply that after executing `y := x - 1`, the [[postcondition]] `y < x` holds (assuming the theory of the natural numbers holds for type `t`).

One of the rules of the wlp calculus is this:

```
wlp(y := e,R) = R[e/y]
```

That is to get the weakest liberal precondition of `R` with respect the the assignment `y := e`, we just substitute `e` for `y` in `R`. In our example above, the verification condition can therefore be written as:

```
x > 0 -> x - 1 < x
```

Since this [[formula]] is valid over the natural numbers, meaning it holds true for any natural number `x`, we conclude that the program is correct. 

In fact, we can check the validity of this formula automatically. 


## WLP calculus and assume/assert

The wlp calculus provides us with rules to cover all of the basic programming constructs in the Ivy language. For example, another way to look at the above example is to consider `requires` and `ensures` as program statements that have a semantics in terms of wlp. When verifying action `decr`, Ivy treats the `requires` statement as an *assumption* and the `ensures` statement as a *guarantee*. This means the program statement we must verify is really:

```
assume x > 0;
y := x - 1;
assert y < x
```

The semantics of the `assume` and `assert` statements are given by:

```
wlp(assume Q, R) = (Q -> R)
wlp(assert Q, R) = (Q & R)
```

That is, we treat `assume Q` as a statement that only terminates if `Q` is true, and `assert Q` as a statement that only succeeds if `Q` is true (that is, if `Q` is false, it does not even satisfy the postcondition `true`). 

## WLP calculus of initializers and invariants

A program (or an [[isolate]]) maintains its [[invariant]] `I` if its [[initializer]] establishes `I` and if each [[export|exported]] [[action]] preserves `I`. Thus, the [[verification condition]] for a program is:

```
wlp(init,I)
```

where `init` is the initializer, and, for each exported action `a`:

```
I -> wlp(a,I)
```

## WLP calculus for other statements

We can compute the wlp of a sequential composition of [[statement|statements]] like this:

```
wlp(S;T, R) = wlp(S,wlp(T,R))
```

To show that our action `decr` satisfies its guarantees, assuming its assumptions,
we compute the wlp of `true`. Computing this for our example using the above rule,
we have:

```
wlp(assert y < x, true) = (y < x)
wlp(y := x -1, y < x) = (x - 1 < x)
wlp(assume x > 0, x - 1 < x) = (x > 0 -> x - 1 < x)
```

which is just what we got before.

Carrying on, we have this rule for conditionals:

```
wlp(if C {T} {E}, R) = ((C -> wlp(T,R)) & (~C -> wlp(E,R)))
```

For a while loop with invariant `I`, the wlp is defined as:

```
wlp(while C invariant I {B}, R) = I
                                & forall mod(B). I & C -> wlp(B,I)
                                & forall mod(B). I & ~C -> R
```

Where `mod(B)` is the list of variables modified in the loop body `B`. This says, essentially, that the invariant must initially hold, that the loop body must preserve the invariant if the entry condition holds, and that otherwise the invariant implies the postcondition.

Notice we haven't dealt with procedure calls here, but for present purposes we can consider that all calls are "in-lined" when verifying the program.


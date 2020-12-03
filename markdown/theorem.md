[[keywords|Keyword]]: `theorem`

One of the [[schema|compound judgments]].

Like [[property|properties]], admitted only with [[proof]]. Essentially the "compound judgment" form of a property, or in other words a "proerty schema". In yet more other words: a property parameterized by *premises* that must be supplied when it is [[judgment application|applied]].

The proof may be generated and checked automatically in simple cases; in more complex cases an explicit proof is required.

## Examples:

### Example: Transitivity of equality
```
theorem [trans] {
    type t
    property [left] X:t = Y
    property [right] Y:t = Z
    #--------------------------------
    property X:t = Z
}
```

This theorem expresses the transitivity of equality. No explicit proof is required here, Ivy's default tactic generates the following [[verification condition]]:

```
X = Y & Y = Z -> X = Z
```

Which Z3 can prove by itself.

### Example: Elimination of universal quantifiers

Here is a theorem that lets us eliminate universal quantifiers:

```
theorem [elimA] {
    type t
    function p(W:t) : bool
    property [prem] forall Y. p(Y)
    #--------------------------------
    property p(X)
}
```

It says, for any predicate `p`, if `p(Y)` holds for all `Y`, then `p(X)` holds for a particular `X`. Again, Z3 can prove this easily.


### Example: triple-idempotence

Now let's derive a consequence of the previous two examples. A function `f` is *idempotent* if applying it twice gives the same result as applying it once. This example theorem says that, if `f` is idempotent, then applying `f` three times is the same as applying it once:

```
theorem [idem2] {
    type t
    function f(X:t) : t
    property [idem] forall X. f(f(X)) = f(X)
    #--------------------------------
    property f(f(f(X))) = f(X)
}
```


The default tactic can't prove this because the premise contains a [[function cycle]] with a universally quantified variable. Here's the error message it gives:

```
error: The verification condition is not in the fragment FAU.

The following terms may generate an infinite sequence of instantiations:
  proving.ivy: line 331: f(f(X_h))
```

This means we'll need to apply some other tactic. Here is one possible proof:
```
proof {
    property [lem1] f(f(f(X))) = f(f(X)) proof {apply elimA with prem=idem}
    property [lem2] f(f(X)) = f(X) proof {apply elimA with prem=idem}
    apply trans with left = lem1
}
```

We think this theorem holds because `f(f(f(X)))` is equal to `f(f(X))` (by idempotence of *f*) which in turn is equal to `f(X)` (again, by idempotence of *f*). We then apply transitivity to infer `f(f(f(X))) = f(X)`.

To prove that `f(f(f(X))) = f(f(X))` from idempotence, we apply the `elimA` theorem with `idem` as the premise. Let's look in a little more detail at how this works. The `elimA` theorem has a premise `forall X. p(X)`. Ivy matches has to find a substitution that will match this formulas `idem`, the premise that we want to use, which is `forall X. f(f(X)) = f(X)`.

It discovers the following substitution:

```
t = t
p(Y) = f(f(Y)) = f(Y)
```
    
 Now Ivy tries to match the conclusion `p(X)` against the property `lem1` we are proving, which  is `f(f(f(X))) = f(f(X))`. It first plugs in the above substitution, so `p(X)` becomes:

```
f(f(X)) = f(X)
```

 Matching this with our goal `lem1`, it then gets this substitution:

```
X = f(X)
```

Plugging these substitutions into `elimA` as defined above, we obtain:

```
theorem [elimA] {
    property [prem] forall Y. f(f(Y)) = f(Y)
    #--------------------------------
    property f(f(f(X))) = f(f(X))
}
```

which is just the inference we want to make. This might give the impression that Ivy can magically arrive at the instantiation of a lemma needed to produce a desired inference. In general, however, this is a hard problem. If Ivy fails, we can still write out the full substitution manually, like this:

```
property [lem1] f(f(f(X))) = f(f(X))
proof {
    apply elimA with t=t, p(Y) = f(f(Y)) = f(Y), X=f(X)
}
```

The second step of our proof, deriving the fact `f(f(X) = f(X)` is a similar application of `idem`.  The final step uses transitivity, with our two lemmas as premises. In this case, we only have to specify the the `left` premise of `trans` is `lem1`. This is enough to infer the needed substitution.  There would be no harm, however, in writing `right=lem2`.

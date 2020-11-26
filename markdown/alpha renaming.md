When [[judgment application|applying judgments]], one sometimes wishes to change the names of [[logical variable|logical variables]] in the applied jugements or premises, to avoid name clashes between introduced variables in a [[proof]].

This can be done by writing an alpha-renaming qualifier, written `<X/Y>` where `X` is the new variable and `Y` is the variable in the formula to alpha-rename.

## Example:

Suppose we have this axiom:

```
relation s(X:t,Y:t)
axiom [prem] forall X. exists Y. s(X,Y)
```

We would like to prove this property:

```
individual a : t
individual b : t
property [sk2] exists Z,W. s(a,Z) & s(b,W)
```

To prove this, we would like to use the axiom twice, once for `X=a` and once for `X=b`. So let’s create two instances of it:

```
proof {
    instantiate [p1] prem with X=a
    instantiate [p2] prem with X=b
    tactic skolemize
    showgoals
}
```

Notice we gave the two instances distinct names, `p1` and `p2`. After [[skolemization|Skolemization]], this is the proof goal we got:

```
#- theorem [prop1] {
#-     individual _Y : t
#-     individual _Y_a : t
#-     property [p1] s(a,_Y)
#-     property [p2] s(b,_Y_a)
#-     property exists W,Z. s(a,Z) & s(b,W)
#- }
```

The Skolem symbol in `p2` was automatically given a fresh name `_Y_a` so as not to clash with the symbol `_Y` used in `p1`. Now we could witness the conclusion with `W=_Y` and `Z=_Y_a`. This is fragile, though, because changes to the proof might result in a name different from `_Y_a`. A better approach is to use alpha renaming to the two quantifiers different variable names. For example:

```
property exists Z,W. s(a,Z) & s(b,W)
proof {
    instantiate [p1] prem<Y1/Y> with X=a
    instantiate [p2] prem<Y2/Y> with X=b
    tactic skolemize
    instantiate with Z=_Y1, W=_Y2
}
```

By renaming the bound variables in our two copies of the axiom to be `Y1` and `Y2`, we can make sure that the Skolem symbols have predictable names. Again, there was no need to use [[explicit quantifier instantiation|manual instantiation]] for this simple problem, but in more complex cases with nested quantifiers, this approach can make it possible to let Z3 do most of the quantifier instantiation work by only instantiating the quantifiers that the fragment checker complains about.

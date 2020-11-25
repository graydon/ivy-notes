A key method of avoiding [[function cycle|function cycles]] (and thus [[recovering decidability]]) is to model a system by [[relation|relations]] rather than [[function|functions]].

This may seem counter-intuitive since mathematically a function is a special case of a relation. But the difference for IVy's purposes occurs when considering the SMT solver's task of instantiating [[quantifier|quantifiers]].

Specifically, if a program has a type `t` with at least one instance `i:t` and a function `f(X:t):t` -- an example of a trivial "function cycle" consisting of a function from `t` to `t` -- then any time an SMT solver wants to instantiate some [[logical variable]] such as `V:t` by plugging in "all possible [[ground term|ground terms]]", it can't: the set is infinite: `i`, `f(i)`, `f(f(i))`, `f(f(f(i)))`, etc. To instantiate quantifiers over such possibly-infinite types, an SMT solver would have to use some more complicated (and fallible) strategy. IVy wants to remain in [[finite almost uninterpreted|FAU]] so that the SMT solver can use the simpler instantiation strategy.

To do so, consider what happens if the program is rewritten to remove `f` and introduce some relation `r(X:t,Y:t)` in its place. Even if the relation **models** the exact same function (say: `r(P,Q)` is true for each `Q` that was the value of `f(P)`), and the relation is used the same way the function was in the program, from the perspective of instantiating a quantifier `V:t` an SMT solver is now back to a finite vocabulary: `V:t` can only take on `i` (or whatever other declared individuals exist in type `t`, such as `j`). The points where the relation `r(X:t,Y:t)` is true no longer figure into quantifier instantiation for `V:t`.

## Example:

Suppose we have the following functions recording a bijective key-value map:

```
type key
type val
function valof(K:key):val
function keyof(V:val):key
action add_key(k:key, v:val) = {
    valof(k) := v;
    keyof(v) := k;
    require forall V:val . valof(keyof(V)) = V
}
export add_key
```

The postcondition has a function cycle for `V:val`, causing the following error:
```
Isolate this:
error: The verification condition is not in the fragment FAU.

The following terms may generate an infinite sequence of instantiations:
  
t.ivy: line 7: new_valof(new_keyof(V)) = V
```

However, an equivalent formulation that uses relations:
```
type key
type val
relation valof(K:key,V:val)
relation keyof(V:val,K:key)
action add_key(k:key, v:val) = {
    valof(k,v) := true;
    keyof(v,k) := true;
    require forall V:val, K:key . keyof(V,K) -> valof(K,V)
}
export add_key
```

Stays within FAU and verifies successfully:
```
Isolate this:

    The following action implementations are present:
        t.ivy: line 7: implementation of add_key

    The following program assertions are treated as assumptions:
        in action add_key when called from the environment:
            t.ivy: line 10: assumption

OK
```

As a bonus, the relational formulation requires fewer relations -- the `keyof` relation is redundant, as there is no image or preimage "directionality" to a relation as there is to a function.
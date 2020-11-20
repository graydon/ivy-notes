A part of an explicit [[proof]].

To use (or "apply") a named [[property]], [[axiom]], [[axiom schema]] or [[theorem]] in a [[proof]], use the [[tactic]] `apply`.

(In some documentation this is also called [[instantiation]] but that term is heavily overloaded so we try to avoid it here.)

[[schemas|Schemas (or "compound judgments")]] are never applied automatically. [[primitive judgments|Primitive judgments]] may be.

Sometimes a schema application can match its premises with terms in the proof environment automatically. But this problem (so-called "second order matching") in general is hard and so some applications require an explicit list of input premises.

## Example:

```
    axiom [congruence] {
        type d
        type r
        function f(X:d) : r
        #--------------------------
        property X = Y -> f(X) = f(Y)
    }
	
    property [prop_n] Z = n -> Z + 1 = n + 1
    proof {
        apply congruence
    }
```

The `proof` declaration tells IVy to apply the [[axiom schema]] `congruence` to prove the [[property]] `prop_n`. 

IVy tries to match the [[proof]] goal `prop_n` to the schema's conclusion by picking particular values for premises, that is, the types `d`,`r` and function `f`. It also chooses terms for the free variables `X`,`Y` occurring in the schema. In this case, it discovers the following assignment:

```
    #- d = t
    #- r = t
    #- X = Z
    #- Y = n
    #- f(N) = N + 1
```

After plugging in this assignment, the conclusion of the rule exactly matches the property to be proved, so the property is admitted.

In case IVy did not succeed in finding the above match, we could also have written the proof more explicitly, like this:

```
    property [prop_n_2] Z = n -> Z + 1 = n + 1
    proof {
        apply congruence with X=Z, Y=n, f(X) = X:t + 1
    }
```

Each of the above equations acts as a constraint on the assignment. That is, it must convert `X` to `Z`, `Y` to `n` and `f(X)` to `X + 1`. Notice that we had to explicitly type `X` on the right-hand side of the last equation, since its type couldn't be inferred (and in fact it's not the same as the type of `X` on the left-hand side, which is `d`). 

It's also possible to write constraints that do not allow for any assignment. In this case, IVy complains that the provided match is inconsistent.

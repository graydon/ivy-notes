Recursive [[definition|definitions]] are permitted in IVy by instantiating a definitional schema. As an example, consider the following [[axiom schema]]:

```
axiom [rec[u]] {
    type q
    function base(X:u) : q
    function step(X:q,Y:u) : q
    fresh function fun(X:u) : q
    #---------------------------------------------------------
    definition fun(X:u) = base(X) if X <= 0 else step(fun(X-1),X)
}
```

This axiom was provided as part of the integer theory when we
interpreted type `u` as `int`.  It gives a way to construct a fresh function `fun` from two functions:

- `base` gives the value of the function for inputs less than or equal to zero.
- `step` gives the value for positive `X` in terms of `X` and the value for `X-1`

A definition schema such as this requires that the defined function symbol be fresh. With this schema, we can define a recursive function that adds the non-negative numbers less than or equal to `X` like this:

```
function sum(X:u) : u

definition {
   sum(X:u) = 0 if X <= 0 else (X + sum(X-1))
}
proof {
   apply rec[u]
}
```
Notice that we wrote the [[definition]] in curly brackets. This causes IVy to treat it as an [[axiom schema]], as opposed to a simple [[axiom]].

We did this because the definition has a universally quantified variable `X` under arithmetic operators, which puts it outside the decidable fragment. Because this definition is a schema, when we want to use it, we'll have to apply it explicitly,

In order to admit this definition, we applied the definition schema `rec[u]`. IVy infers the following substitution:

```
q=t, base(X) = 0, step(X,Y) = Y + X, fun(X) = sum(X)
```

This allows the recursive definition to be admitted, providing that `sum` is fresh in the current context (i.e., we have not previously referred to `sum` except to declare its type).

### Extended matching

Suppose we want to define a recursive function that takes an additional parameter. For example, before summing, we want to divide all the numbers by `N`. We can define such a function like this:

```
function sumdiv(N:u,X:u) : u

definition
    sumdiv(N,X) = 0 if X <= 0 else (X/N + sumdiv(N,X-1))
proof
    apply rec[u]
```

In matching the recursion schema `rec[u]`, IVy will extend the function symbols in the premises of `rec[u]` with an extra parameter `N` so that schema becomes:

```
axiom [rec[u]] = {
    type q
    function base(N:u,X:u) : q
    function step(N:u,X:q,Y:u) : q
    fresh function fun(N:u,X:u) : q
    #---------------------------------------------------------
    definition fun(N:u,X:u) = base(N,X) if X <= 0 else step(N,fun(N,X-1),X)
}
```
The extended schema matches the definition, with the following assignment:

```
q=t, base(N,X)=0, step(N,X,Y)=Y/N+X, fun(N,X) = sum2(N,X)
```

This is somewhat as if the functions were "curried", in which case the free symbol `fun` would match the partially applied term `sumdiv N`. Since IVy's logic doesn't allow for partial application of functions, extended matching provides an alternative. Notice that, to match the recursion schema, a function definition must be recursive in its *last* parameter.

Induction
=========

The default tactic can't generally prove properties by induction. For that IVy needs manual help. To prove a property by induction, we define an invariant and prove it by instantiating an induction schema. Here is an example of such a schema, that works for the non-negative integers:

```
axiom [ind[u]] {
    relation p(X:u)
    theorem [base] {
        individual x:u
        property x <= 0 -> p(x)
    }
    theorem [step] {
        individual x:u
        property p(x) -> p(x+1)
    }
    #--------------------------
    property p(X)    
}
```

Like the recursion schema `rec[u]`, the induction schema `ind[u]` is part of the integer theory, and becomes available when we interpret type `u` as `int`.

Suppose we want to prove that `sum(Y)` is always greater than or equal to `Y`, that is:

```
property [sumprop] sum(Y) >= Y
```

We can prove this by applying our induction schema. Here is a backward version of the proof:

```
proof [sumprop] {
    apply ind[u] with X = Y
    proof [base] {
        instantiate sum with X = x
    }
    proof [step] {
        instantiate sum with X = x + 1
    }
}
```

Applying `ind[u]` produces two sub-goals, a base case and an induction step:

```
theorem [base] {
    individual x:u
    property x <= 0 -> sum(x) >= x
}

theorem [step] {
    individual x:u
    property [step] sum(x) >= x -> sum(x+1) >= x + 1
}
```

The default tactic can't prove these goals because the definition of `sum` is a schema that must explicitly instantiated. Fortunately, it suffices to instantiate this schema just for the specific arguments of `sum` in our subgoals. For the base case, we need to instantiate the definition for `X`, while for the induction step, we need `X+1`. The effect of the `instantiate` tactic is to add an instance of the definition of `sum` to the proof context, so in particular, it will be used by the default tactic.

Notice that we referred to the definition of `sum` by the name
`sum`.  Alternatively, we can name the definition itself and refer
to it by this name.

After instantiating the definition of `sum`, our two subgoals look like this:

```
theorem [prop5] {
    property [def2] sum(Y + 1) = (0 if Y + 1 <= 0 else Y + 1 + sum(Y + 1 - 1))
    property sum(Y) >= Y -> sum(Y + 1) >= Y + 1
}


theorem [prop4] {
    property [def2] sum(Y) = (0 if Y <= 0 else Y + sum(Y - 1))
    property Y:u <= 0 -> sum(Y) >= Y
}
```

Because these goals are quantifier-free the default tactic can easily handle them, so our proof is complete.

As an alternative, here is the slightly more verbose forward version of this proof:

```
property [sumprop2] sum(Y) >= Y

proof [sumprop2] {
    theorem [base] {
        individual x:u
        property x <= 0 -> sum(x) >= x
    } proof {
        instantiate sum with X = x
    }
    theorem [step] {
        individual x:u
        property sum(x) >= x -> sum(x+1) >= x + 1
    } proof {
        instantiate sum with X = x + 1
    }
    apply ind[u]
}
```

This version is more clear than the backward version in that the base case and inductive step of the proof are stated explicitly. For a complex inductive hypothesis, this style might get a bit verbose. To reduce the amount of text we need to type, we can introduce a function definition inside a proof, like this:

```
theorem [foo] {property sum(X) >= X}
proof {
    function inv(X) = sum(X) >= X
    property inv(X) proof {
        theorem [base] {
            individual x:u
            property x <= 0 -> inv(x)
        } proof {
            instantiate sum with X = x
        }
        theorem [step] {
            individual x:u
            property inv(x) -> inv(x + 1)
        } proof {
            instantiate sum with X = x + 1
        }
        apply ind[u]
    }
}
```

The concept of **relevant vocabularies** extends the concept of [[stratification]]. Both allow extending [[effectively propositional|EPR]] with more [[ground term|ground terms]] (including some functions) while remaining decidable.

Following [Ge and de Moura](http://leodemoura.github.io/files/ci.pdf), we will define a **relevant vocabulary** for every [[universal quantifier]] and for each argument of every [[uninterpreted function]] or predicate symbol. The idea is that, if we instantiate each universal quantifier with just the [[ground term|ground terms]] in its relevant vocabulary, then any model of these instances can be converted to a model of the original formula. If the relevant vocabulary is finite, the formula is in the decidable fragment.

To determine the relevant vocabularies, we have a set of rules that depend on the formula. We will say that `V[X]` is the relevant vocabulary of universally quantified variable `X` and `V[f,i]` is the relevant vocabulary of argument `i` of uninterpreted function or predicate symbol `f`.  The rules are in one of two forms:

- If vocabulary `V` contains term `t` then vocabulary `W` contains term `u`, or
- Vocabulary `V` equals vocabulary `U`.

The relevant vocabularies are the least ones that satisfy all the rules. To express the rules, we use `t[X1...XN]` to stand for a term with free variables `X1...XN` (so if N=0, it is a ground term). We use `t[V1...VN]` to stand for any instantiation of `t` using the
vocabularies `V1...VN`.

For first-order logic, we have the following rules:

- if `t[X1...XN]` is the `i`th argument of `f`, every instance `t[V[X1],...,V[X1]]` is in `V[f,i]`
- if universal variable `X` is the `i`th argument of `f`, then `V[X] = V[f,i]`.

To handle equality, we introduce a vocabulary `V[t]` for each type `t` and add these rules:

- If `X:u = e` or `e = X:u` occurs, then `V[X] = V[u]`,
- If `t[X1...XN]:u = e` or `e = t[X1...XN]:u`  occurs then every instance `t[V[X1],...,V[XN]]` is in `V[u]`.

## Finiteness and cycles

To determine if the relevant vocabulary is finite, we don't need to compute it. It suffices to determine if the rules create a cycle that can generate an infinite set of terms.  To do this, we build a graph whose nodes are vocabularies and whose arcs are defined by the
following rules:

| term                 |  arc(s)         |
|----------------------|-----------------|
| `f(..,X,..)`           | `V[X] <-> V[f,i]` |
| `f(..,t[..,X,..],..)`  | `V[X] -> V[f,i]`  |
| `X:u = e`              | `V[X] <-> V[u]`   |
| `t[...,X:u,...] = e`   | `V[X] -> V[u]`    |

Where `i` is the relevant argument position in `f` and `x = y` is considered equivalent to `y = x`.

IVy constructs this graph for each negated VC. If the graph has a cycle, Ivy reports the sequence of terms that induced the cycle (and gives references to line numbers in the IVy program). This information can be used to determine the source of the problem and and [[recovering decidability|correct it]].


## Examples:

### Example: finite relevant vocabularies, without equality rules

Consider the conjunction of the following formulas:

```
r(f(a),c)
forall X. r(f(X),X)
```

The relevant vocabularies are:

```
V[f,1] = {a,c}
V[r,1] = {f(a),f(c)}
V[r,2] = {a,c}
V[X] = {a,c}
```

We can arrive at this result by just applying the rules until we reach a fixed point. From the first rule, we have `V[f,1] = {a}` and `V[r,2] = {c}`. Then, from the second rule, because *X* occurs in argument position 1 of *f* and 2 of *r*, we have `V[f,1] = V[r,2] = V[X] = {a,c}`.  The first rule then gives us `V[r,1] = {f(a),f(c)}` and we have reached a fixed point.

Since the relevant vocabularies are finite, this conjunction is in the decidable fragment.


### Example: finite relevant vocabularies, with equality

Suppose `f` is a function from `u` to `u` and we have this conjunction:

```
f(a) = c
forall X. r(f(X),X)
```

The relevant vocabularies are:

```
V[u] = {f(a),c}
V[f,1] = {a}
V[r,1] = {f(a)}
V[r,2] = {a}
V[X] = {a}
```

That is, even though the function `f` is not [[stratification|stratified]], we still need only a finite instantiation to determine satisfiability of this formula. It is easy seen, however, that all the stratified formulas have finite relevant vocabularies.

### Example: infinite (cyclical) relevant vocabulary

As an example, consider the following formula:

```
forall X. r(X,a) -> r(f(X),X)
```

There is a cycle `V[X] -> V[f,1] -> V[r,1] -> V[X]` induced by these terms:

```
f(X)
r(f(X),X)
r(X,a)
```

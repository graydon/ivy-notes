The concept of **stratification** allows us to extend [[effectively propositional|EPR]] with some [[ground term|ground terms]] (including some functions) while remaining decidable.

## Stratified function symbols

[[effectively propositional|EPR]] is a very restrictive logic, since in effect it only allows us to say that something exists if it has an explicit name. We can go a bit further by adding **stratified function symbols**. For example, suppose we define the following vocabulary of functions and constants:

```
individual x : t
function f(X:t) : u
function g(Y:u) : v
```

Using this vocabulary, we can only generate three [[ground term|ground terms]]: `x`, `f(x)`, `g(f(x))`. This means the [[quantifier alternation|EA formulas]] using this vocabulary are decidable.

In general, suppose we construct a directed graph `(V,E)` where the vertices `V` are the [[type|types]], and we have an edge `(t,u)` in `E` whenever there is a [[function]] from `... * t * ...` to `u`. The function symbols are **stratified** if there is no cycle in this graph (including trivial cycles from `t` to `t`). Stratified [[quantifier alternation|EA formulas]] are in the [[logical fragment|decidable fragment]]. Since the axioms of equality are in EPR, the equality symbol is also allowed.

Using stratified function symbols is an important strategy for keeping [[verification condition|verification conditions]] in the decidable fragment. When planning the specification of a system, it is useful to carefully choose an order on the types, so that it is possible to use only functions from lesser to greater types. When a functions from types `t` to `u` and `u` to `t` are both needed, the best practice is to separate these function symbols by confining them to different [[isolate|isolates]].

## Stratified [[quantifier alternation|quantifier alternations]]
Ultimately we need to be able to write formulas in [[quantifier alternation|AE form]]. That is, we want to say things like “for every epoch E there exists a leader L”. When these formulas occur [[positive and negative occurrences|positively]] (as [[assumption|assumptions]]) they are not in EPR. However, the decidable fragment still contains a limited subset of such formulas.

To see this, we need to understand how AE formulas are handled when determining satisfiability. This is done using a transformation called [[skolemization]]. The formula `forall X. exists Y. p(X,Y)` is satisfiable if and only if `forall X. p(X,f(X))` is satisfiable for a fresh function symbol `f` called a Skolem function. This means that we can always eliminate all of the existential quantifiers from the negated VC. However, the Skolem functions must also be considered with regard to decidability. That is, if the set of all function symbols, including Skolem functions, is stratified, then the formula is in the decidable fragment. Another way to think of this is that the quantifier sequence `forall X:t. exists Y:u` induces an arc from `t` to `u` in the type graph.

In fact, we can be a little more liberal than this. Consider the formula:

```
forall X:t,Y:u. q(X,Y) -> exists Z:v. p(X,Z)
```

There is no arc induced from type `u` to type `v`, since the variable `Y` does not occur under the existential quantifier, and thus the Skolem function does not depend on it.

Because of stratified quantifier alternations, the negated VC of the following program is in the decidable fragment:

```
require forall X:t. exists Y:u. r(X,Y);
if f(x) = y {
    r(x,y) := true
};       
ensure forall X:t. exists Y:u. r(X,Y)
```

The [[precondition]] occurs positively, so in introduces an arc from `t` to `u`. The [[postcondition]] occurs negatively, so it is an EA formula. Since there are no other arcs in the type graph, it is acyclic, so the negated VC is stratified.

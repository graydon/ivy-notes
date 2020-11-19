IVy works with several different fragments of [first-order logic](https://en.wikipedia.org/wiki/First-order_logic)

The most important fragment in IVy is [FAU ("Finite Almost Uninterpreted")](http://leodemoura.github.io/files/citr09.pdf) which subsumes other decidable fragments including [stratified many-sorted logic](http://www.cs.tau.ac.il/~msagiv/lpar07.pdf) and [EPR ("Effectively-PRopositional") logic](https://en.wikipedia.org/wiki/Bernays%E2%80%93Sch%C3%B6nfinkel_class).

The underlying solver Z3 supports a complete decision procedure, based on quantifier instantiation, for FAU.


## Concepts

Quoting directly from the paper [Paxos Made EPR](https://theory.stanford.edu/~padon/paxos-made-epr-oopsla17.pdf)", and others:

  - **EPR**:  a fragment of first-order logic restricted to relational first-order formulas (i.e., formulas over a vocabulary that contains constant symbols and relation symbols but **no function symbols**) with a quantifier prefix ``∃∗∀∗`` in prenex normal form. Satisfiability of EPR formulas is decidable. Moreover, formulas in this fragment enjoy the finite model property, meaning that a satisfiable formula is guaranteed to have a finite model. The size of this model is bounded by the total number of existential quantifiers and constants in the formula. The reason for this is that given an `∃∗∀∗`-formula, we can obtain an equi-satisfiable quantifier-free formula by Skolemization, i.e., replacing the existentially quantified variables by constants, and then instantiating the universal quantifiers for all constants.

  - **[Many-sorted logic](https://en.wikipedia.org/wiki/First-order_logic#Many-sorted_logic)**: an extension of the domain of discourse of first-order logic into "sorts", analogous to "types" in programming. The sorts of many-sorted logic are the [[type|types]] of IVy.
  - **[Skolemization](https://en.wikipedia.org/wiki/Skolem_normal_form)**: The process of replacing existentially-quantified _variables_ with _functions_ of existing univerally-quantified variables, producing an equi-satisfiable formula with prenex consisting strictly of universal quantifiers. As function symbols in first-order formulae are _implicitly_ existentially quantified, this essentially "hoists existential quantifiers" out of the inside of a formula, moving them to the outermost level where they quantify over functions.
  - **Quantifier-alternation graph**: Let `φ` be a formula in negation normal form over a many-sorted signature `Σ` with a set of sorts `S`. We define the quantifier-alternation graph of `φ` as a directed graph where the set of vertices is the set of sorts, `S`, and the set of directed edges, called `∀∃` edges, is defined as follows.
    - Function edges: let `f` be a function in `φ` from sorts `s1,...,sk` to sort `s`. Then there is a `∀∃` edge from `si` to `s` for every `1 ≤ i ≤ k`.
    - Quantifier edges: let `∃x : s` be an existential quantifier that resides in the scope of the universal quantifiers `∀x1 : s1,. . . ,∀xk : sk` in `φ`. Then there is a `∀∃` edge from `si` to `s` for every `1 ≤ i ≤ k`.
    
    Intuitively, the quantifier edges correspond to the edges that would arise as function edges if Skolemization is applied.
	
  - **Stratification**: A vocabulary for many-sorted logic is _stratified_ if there is a function `level` from sorts (types) into `Nat` such that for every function symbol `f : A1 × ... × Am → B` we have `level(B) < level(Ai) for all i = 1,...,m`. Alternatively: A formula `φ` is stratified if its quantifier alternation graph is acyclic.

  - **Extended EPR**. A formula `φ` is stratified if its quantifier alternation graph is acyclic, **including functions, so long as they are stratified**. The extended EPR fragment consists of all stratified formulas. This fragment maintains the finite model property and the decidability of EPR. The reason for this is that, after Skolemization, the vocabulary of a stratified formula can only generate a finite set of ground terms. This allows complete instantiation of the universal quantifiers in the Skolemized formula, as in EPR.

FAU extends this Extended EPR in a few more ways related to arithmetic and array theories that we will not cover here presently.

## Example:

Since Skolemization replaces quantifiers with functions, the concept of "stratification" applies equally to functions and/or quantifier-alternations. For example:

  - Initial formula `∀x:t . ∃y:u . F(x,y)`
  - Skolemizes as `∃S:t->u . ∀x:t . F(x,S(x))`

Looking at the Skolemized case, we can see that the definition of stratification has an "edge in the graph" of sorts (types) between `t` and `u` provided by the skolem function `S`.

Similarly, in the initial (non-Skolemized) case, the `∀x:t . ∃y:u` quantifier-alternation can also be viewed as a different representation of the same "edge" in the graph of sorts.
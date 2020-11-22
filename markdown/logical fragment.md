IVy works with several different fragments of [[first-order logic]].

A fragment is just a restriction of the full set of possible [[term|terms]] -- some terms are excluded. The trick is in choosing the set to exclude.

The most important fragment in IVy is called [[finite almost uninterpreted|FAU]]. By default, IVy checks that every [[verification condition]] is in FAU and rejects those that fall outside of it. Understanding FAU is fairly important to understanding how to [[recovering decidability|recover decidability]] in an IVy program, when the fragment checker rejects it.

The underlying solver Z3 supports a complete decision procedure, based on quantifier instantiation, for FAU.

See also some of the later papers in [[further info]].

## Fragments

The fragment FAU is best understood by building up from simpler fragments, adding small extensions that each preserve decidability:

  - The [[effectively propositional]] fragment (EPR) is simplest
  - The [[finite essentially uninterpreted]] fragment (FEU) extends EPR
  - The [[finite almost uninterpreted]] fragment (FAU) extends FEU

## Concepts

The following concepts are used in explaining the fragments above. They are collected here for more direct exploration/reference:

  - [[first-order logic]]
  - [[many-sorted logic]]
  - [[ground term]], [[quantifier]] and [[logical variable]]
  - [[theory instantiation]] a.k.a. "interpretation"
  - [[quantifier alternation]]
  - [[skolemization]]
  - [[stratification]]
  - [[relevant vocabulary]]
  - [[arithmetic literal]]

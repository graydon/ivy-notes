IVy’s [[logical fragment|decidable fragment]] consists of all the formulas that are in FAU after full [[unfold|unfolding]] of all the non-[[recursion|recursive]] [[definition|definitions]].

When a [[verification condition]] is not in the decidable fragment, IVy refuses to check it, and instead produces an error message explaining the problem. This explanation is generally either:
  - A list of terms that induce a cycle in the [[relevant vocabulary]] graph.
  - An instance of an [[theory instantiation|interpreted symbol]] applied to universal variable that is not an [[arithmetic literal]].

There are several approaches to correcting such a problem:

  - Change the encoding of the state of the system to [[use relations instead of functions]].
  - [[visibility qualifiers|Use modularity to hide a function symbol or theory]] that is problematic.
  - Use proof tactics to transform the property to be proved:
      - [[judgment application|Apply an existing judgment]] that may not be automatically applied.
	  - [[skolemization|Skolemize premises]] to eliminate their existential quantifiers.
	  - [[explicit quantifier instantiation|Explicitly instantiate]] remaining quantifiers.

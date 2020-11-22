[[keywords|Keyword]]: `instantiate`

Formulas containing [[universal quantifier|universal]] and [[existential quantifier|existential]] quantifiers may be instantiated, providing [[ground term|ground terms]] for one or more of their [[logical variable|logical variables]].

This can be done in a [[proof]] using the `instantiate` [[tactic]].

Specifically:

  - An existential quantifier at the outer level of the **conclusion** of a proof goal may be instantiated while preserving logical implication. After all, if the proof goal is to show that given the premises, "there exists" some term that makes the rest of the conclusion true, then supplying such a term -- called a **witness** -- clearly satisfies the quantifier.
  - A universal quantifier at the outer level of any **premise** of a proof goal may also be instantiated while preserving logical implication. This is because: if a given premise holds "for all" substitutions of a logical variable, then it is also true for a single chosen value. Instantiating a variable in a premise essentially weakens the premise; but may make it easier to match with a chosen witness in the conclusion.

Existential quantifiers in premises may be eliminated by [[skolemization]] first, which may make it possible to denote a witness in terms of the resulting Skolem functions.

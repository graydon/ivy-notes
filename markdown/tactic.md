A tactic is a step that can be invoked by name during a [[proof]].

There is a default tactic called `tactic vcgen` that works by generating a [[verification condition]] (VC) to be checked by Z3. This is a formula whose validity implies that the property is true in the current context. IVy checks that this formula is within the [[logical fragment]] FAU that Z3 can decide, then passes the formula to Z3. If Z3 finds that the formula is valid, the property is admitted.

If the default tactic generates a VC that is not within FAU, IVy will describe the problem and user intervention will be required, invoking other tactics to arrive at a goal state that *is* within FAU.

Possible tactics include:
  - `apply`, `instance`, or `instantiate` applies [[schema|compound judgments]] or `explicit` [[primitive judgment|primitive judgments]] and optionally also [[explicit quantifier instantiation|instantiates quantified variables]]
  - `let` performs [[explicit quantifier instantiation]] on existing goals
  - `showgoals` prints the current list of proof goals
  - `defergoal`
  - `assume` adds an [[assumption]]
  - `unfold`
  - `forget`
  - `spoil`
  - `property`, `axiom`, etc. allow declaring (and proving) a local sub-[[logical judgment|judgment]]
  - `tactic` invokes extended, non-keyword tactics:
    - `tactic skolemize` performs [[skolemization|Skolemization]]
    - `tactic vcgen` is the default VC-generating tactic
    - `tactic l2s` translates [[temporal properties]] about liveness to safety properties
    - `tactic invariance` also related to [[temporal properties]]

A [[function]] is **uninterpreted** when it is defined on [[uninterpreted type|uninterpreted types]].

This means that there is no additional theory associated with the function's input and output types (either providing or constraining possible inputs or outputs) beyond [[declaration|declarations]] and [[definition|definitions]] provided by the user.

Many functions in IVy programs are uninterpreted.

All a solver knows about an uninterpreted function is:
  - The function **exists**.
  - It can be applied to terms of its input type.
  - Such applications are terms of its output type.
  - Occurrences of the function must satisfy other properties or axioms declared by the user.

Importantly: satisfiability of terms in the theory of [uninterpreted functions](https://en.wikipedia.org/wiki/Uninterpreted_function) ("UF") is decidable, and all SMT solvers include a decision procedure for it (called "unification").

This fact -- that satisfiability of UF is decidable -- may seem surprising or counterintuitive because solvers "know so little" about the functions in UF. They may be anything!

But the point is that UF itself is a small theory -- it doesn't include basic arithmetic -- and since the types involved in an uninterpreted function are themselves uninterpreted, there are:
  - very few **possibilities** for [[ground terms]] of an uninterpreted type serving as input to the function (only those [[individual|individuals]] explicitly declared, or other applications of [[uninterpreted function]]) 
  - very few possible **constraints** that might occur in a UF formula that would cause a given candidate model not to satisfy the formula

So simple decision procedures like "plug in all possible ground terms" work.

In general, more-powerful theories become less decidable; and UF is not a very powerful theory.

UF is a sub-theory of all the [[logical fragment|fragments]] used in IVy.
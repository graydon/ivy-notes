A "sort" is formal-logic jargon for the same concept as a "type" in programming languages, generally. It denotes a set of objects in the domain of discourse for [[many-sorted logic]].

In IVy's case, "sorts" denote **specifically** the sorts that are available for interpretation by the underlying SMT solver Z3. There are not many of these: `int`, `nat`, enumerated sorts, subrange sorts and bitvector sorts.

User-defined types in IVy can be **interpreted** as such a sort using [[theory instantiation]], but usually it's desirable to *avoid* such interpretation, as it risks giving "too much theory" to any [[expressions]] using the type, such that any resulting [[verification condition|verification conditions]] fall outside the FAU [[logical fragment]] that Z3 can decide without help.
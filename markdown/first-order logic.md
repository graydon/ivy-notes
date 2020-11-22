First-order logic is an extension of preciate logic to include the [[universal quantifier|universal]] and [[existential quantifier|existential]] quantifiers, potentially over multiple disjoint sets called [[sort|sorts]].

Satisfiability in first-order logic is only semi-dedicable: procedures exist that can enumerate the space of possible satisfying assignments for a formula, but that space is both infinite and sufficiently rich that such procedures may fail to terminate with any answer.

IVy reasons primarily over a [[logical fragment|restricted fragment]] of first-order logic called FAU. 

Though IVy's [[expression|expressions]] can denote any term in unrestricted first-order logic, some [[verification condition|verification conditions]] will be rejected if they lie outside FAU. The user must then [[recovering decidability|recover decidability]] of the VC.

When running `ivy_check` in `complete=fo` mode, IVy will attempt to form and check [[verification condition|verification conditions]] in unrestricted first-order logic. This may cause checking to fail to terminate.

First-order logic does not allow writing formulas that quantify over other formulas. Such quantification is only possible in [second-order](https://en.wikipedia.org/wiki/Second-order_logic) and [higher-order logic](https://en.wikipedia.org/wiki/Second-order_logic). Instead, in first-order logic one may write and prove a [[schema]] in the surrounding metalanguage -- a language like IVy's language of declarations, proofs and tactics, **outside** the logic -- and explicitly instantiate the schema's premises with formulas.

See also the [wikipedia page](https://en.wikipedia.org/wiki/First-order_logic) on first-order logic.
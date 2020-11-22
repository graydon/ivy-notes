One way of characterizing the complexity of various decision problems in formal logic involves looking at the nesting structure of the quantifier prefix in their [[prenex normal form]].

In particular, **alternations** in the nesting structure are relevant: positions where one or more [[universal quantifier|universal quantifiers]] are followed by one or more [[existential quantifier|existential quantifiers]], or vice-versa.

This is a fairly large topic itself but for our purposes, it's only relevant that:
  - The number and order of quantifier alternations matter to the complexity of analyzing a formula. The restrictions of the various [[logical fragment|logical fragments]] of interest in IVy are almost all about limiting discourse to specific alternations (for example [[effectively propositional|EPR]] is `exists.forall.`)
  - Depending on the writer, the alternation structure of a formula may be abbreviated a few equivalent ways:
    - The universal quantifier is written `forall` in IVy but `∀` in formal logic and simply `A` in a fair amount of literature. That is, a writer might refer to an "A formula".
    - The existential quantifier is written `exists` in IVy but `∃` in formal logic and imply `E` in a fair amount of literature. That is, a writer might refer to an "E formula".
    - Therefore people may write about, for example, `∀∃∀` formulas for those formulas that have a prefix that alternates between universal, existential, and finally universal quantification. Other authors might just call this an "AEA formula".
    - In many contexts, sequences of quantifiers of the same type (universal or existential) are abbreviated by an asterisk, for example `∀*∃∀` might refer to formulas that have **multiple** universal quantifiers before the first alternation to an existential quantifier. In other contexts the asterisks might be omitted, and an `∀∃∀` formula might really mean "any formula with any number of repetitions of each quantifier type".

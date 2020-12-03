[[keywords|Keyword]]: `ghost`

The term "ghost" or "ghost code" refers to any code that has logical content (for purposes of specification and verification) but which is erased from an executable implementation (eg. during [[code extraction]] as a verified C++ program).

Since ghost code exists strictly for logical purposes, there are no requirements that it be efficient, use bounded storage, or otherwise be "realistic". For example, it is common for ghost code in a verified program implementing a protocol to retain a ghost list (or function, relation, etc.) of every message the program ever receives. Such a ghost data structure can then serve as a term in [[verification condition|verification conditions]] -- for example, in an assertion about values in it or their order -- even though it will never really be kept in-memory in an executable version of the program.

In Ivy, the keyword `ghost` can prefix a [[type]] declaration to indicate that [[state variable|state variables]] and [[expression|expressions]] of the type should be considered ghost code and erased during [[code extraction]].
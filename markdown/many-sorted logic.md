Many-sorted logic is an extension of the domain of discourse of [[first-order logic]] into "[[sort|sorts]]", analogous to "types" in programming.

The "sorts" of many-sorted logic are the [[type|types]] of Ivy.

By default, a type in Ivy is an [[uninterpreted type]]. The underlying solver Z3 can reason directly over these types and the associated theory of [[uninterpreted function|uninterpreted functions]].

Types can also be [[theory instantiation|given an interpretation]] in terms of one of the other sorts known to Z3.

Separately-declared types in Ivy are treated as disjoint (though potentially uninterpreted) sorts in the underlying logic. This is true even of separately-declared types with the same interpretation: if two separately-declared types `A` and `B` are both interpreted as `int`, they are still separate: terms of type `A` cannot be used where terms of type `B` are required, or vice-versa.

See also [wikipedia's page](https://en.wikipedia.org/wiki/Many-sorted_logic) on many-sorted logic.
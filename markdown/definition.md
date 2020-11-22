[[keywords|Keyword]]: `definition`

One of the [[primitive judgment|primitive judgments]].

A special form of [[axiom]] that cannot introduce an inconsistency.

Definitions can also be provided in-line with the declaration of a [[function]] or [[relation]], by following the declaration with an equals sign (`=`) and an [[expression]].

Defining a [[function]], [[relation]] or [[declared variable]] changes it from being a [[state variable]] to being immutable. Immutable definitions cannot be [[assignment|assigned]] in [[action|actions]].

## Example:
```
function g(X:t) : t

definition g(X) = f(X) + 1
```

This can be read as "for all `X`, let `g(X)` equal `f(X) + 1`.

It can be written, equivalently, in its in-line form as:

```
function g(X:t) : t = f(X) + 1
```

The definition is admissible if the symbol `g` is "fresh", meaning it does not occur in any existing properties or definitions in the current context. 

Further `g` must not occur on the right-hand side of the equality (that is, a recursive definition is not admissible without [[proof]] -- see [[recursion]]).

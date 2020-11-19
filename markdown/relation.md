[[keywords|Keyword]]: `relation`

One of the [[primitive judgment|primitive judgments]].

Declares the existence of a relation. Optionally, if followed by an equality, provides a definition.

A relation that is declared but not defined is a [[state variable]] that may have its values partially or fully [[assignment|assigned]] -- point-wise -- during [[action|actions]].

If defined, a relation is immutable.

## Example:
```
relation r(X:t,Y:u)
```

Where `t`and `u` are some  [[type|types]].

This judgment can be read as "for any terms `X` of type `t` and `Y` of type `u`, let `r(X,Y)` be a [[proposition]].
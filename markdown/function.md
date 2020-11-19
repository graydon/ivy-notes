[[keywords|Keyword]]: `function`

One of the [[primitive judgment|primitive judgments]].

Declares the existence of a function. Optionally, if followed by an equality, provides a definition.

A function that is declared but not defined is a [[state variable]] that may have its values partially or fully [[assignment|assigned]] -- point-wise -- during [[action|actions]].

If defined, a function is immutable.

## Example:
```
function f(X:t) : u
```

Where `t`and `u` are some [[type|types]].

This judgment can be read as "for any term `X` of type `t`, let `f(X)` be a term of type `u`".
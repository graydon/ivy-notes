A nondeterministic choice combines a [[choice function]] with a [[nondeterministic conditional]].

Its syntactic form is thus  `if some <symbol> . <constraint> { ... }`

If the choice function succeeds, and some `<symbol>` does exist that meets `<constraint>`, the conditional `if` branch is taken (binding a value nondeterministically), otherwise it is not taken (and any optional `else` branch is taken).

If it also possible to choose a value in such a conditional either `minimizing` or `maximizing` some function of the value.


## Examples:

### Example: nondeterministic conditional

```
if some x:t. f(x) = y {
    z := x + y
} else {
    z := y
}        
```

Here, if there is any value `x` of type `t` such that `f(x) = y`, then such a value is assigned to `x` and the assignment `z := x + y` is executed. If there is more than one such value, the choice is non-deterministic. If there is no such value, the `else` clause is executed. The symbol `x` is only in scope in the `if` clause. It acts like a [[declared variable|local variable]] and is distinct from any `x` declared in an outer scope. 

### Example: nondeterministic conditional with minimizing

For example, we can find an element of a set `s` with the least key like this:

```
if some x:t. s(x) minimizing key(x) {
    ...
}
```

This is logically equivalent to the following:

    if some x:t. s(x) & ~exists Y. s(Y) & key(Y) < key(x) {
       ...
    }

Besides being more concise, the `minimizing` syntax can be more efficiently compiled and is easier for Ivy to reason about. The keyword `maximizing` produces the same result with the direction of `<` reversed.
A nondeterministic [[conditional]] is one of the forms of [[nondeterminism]] in Ivy.

Such a conditional takes the `if` (or optional `else`) branch nondeterministically. An asterisk (`*`) is used as a placeholder for the condition on which the branch depends.


## Example:

```
if * {
    x := y
} else {
    x := z
}
```

In this example, the state variable `x` is assigned either `y` or `z`, nondeterministically.
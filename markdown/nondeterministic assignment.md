Nondeterministic [[assignment]] is one of the forms of [[nondeterminism]] in Ivy.

Such an assignment assigns an arbitrary value to a state variable. An asterisk (`*`) is used as a placeholder for the arbitrary value assigned.

## Example:

```
var x:t
action foo = {
   x := *
}
```


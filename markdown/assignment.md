Assignment is a primitive [[statement]] that can occur within an [[action]].

It is denoted by the `:=` operator.

Assignments may modify multiple state variables simultaneously, in order to capture multiple return values from a [[call|called]] [[action]].

The right-hand side of an assignment may be any of:
  - An [[expression]], causing assignment of concrete values
  - A [[nondeterministic choice]] (`*`) operator, causing an arbitrary assignment
  - A [[call]] to another [[action]], causing assignment of the action's return value(s)

Assignments may make use of [[free variable|free variables]] to assign multiple values within a [[function]] or [[relation]] simultaneously.

Assignment may also be combined with the [[declared variable|declaration of a local variable]].

## Examples:

### Example: State variable assignment

```
var x:bool

action foo = {
    x := true
}
```

### Example: relation assignment

```
type t = {red, green, blue}

relation bar(X:t, Y:t)

action foo = {
    bar(red,blue) := true
}
```

### Example: call assignment
```
action foo returns (x:bool) = {
    x := true
}

var y: bool
action bar = {
    y := foo
}
```

### Example: multi-value call assignment
```
action foo returns (a:bool, b:bool) = {
    a := true;
    b := false
}

var x: bool
var y: bool

action bar = {
    (x, y) := foo
}
```

### Example: nondeterministic choice
```
type t = {red, green, blue}

relation bar(X:t, Y:t)

action foo = {
    bar(red,blue) := *
}
```

### Example: free variable assignment

```
type t = {red, green, blue}

relation bar(X:t, Y:t)

action foo = {
    bar(P, Q) := true
}
```
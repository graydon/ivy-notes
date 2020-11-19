[[keywords|Keyword]]: `var`

Variables are a type of declaration (so can be declared at [[module]] level) but they can also occur as a [[statement]] locally within an [[action]].

A variable can be [[definition|defined]] using a separate `definition` declaration, which will make it immutable.

If a variable is not immutable, it is a [[state variable]] that can be [[assignment|assigned]] during an action.

## Examples:

### Example: immutable variable

```
type color = {red, green, blue}
var v: color
definition v = red
```

This [[definition|defines]] an immutable variable `v` of type `color` with value `red`.

### Example: mutable variable
```
type color = {red, green, blue}
var v: color

action change_to_green = {
   v := green
}
```

This [[declaration|declares]] a [[state variable]] `v` of type `color` and an action that [[assignment|assigns]] the value `green` to `v`.

### Example: local variable
```
type color = {red, green, blue}

action set_local_green = {
   var v: color;
   v := green
}
```

This [[declaration|declares]] a _local_ variable `v` of type `color` inside an action that [[assignment|assigns]] the value `green` to `v`.

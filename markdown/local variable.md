[[keywords|Keyword]]: `local`

A local variable is a [[statement]] that introduces a new [[state variable]] inside an [[action]].

A local variable carries an associated block that indicates the scope of the variable.

## Example:

This [[declaration|declares]] a local variable `v` of type `color` inside an action that [[assignment|assigns]] the value `green` to `v`.

```
type color = {red, green, blue}

action set_local_green = {
   local v:color {
       v := green
   }
}
```

It is identical to the following action, using a [[declared variable]]:

```
type color = {red, green, blue}

action set_local_green = {
   var v: color;
   v := green
}
```
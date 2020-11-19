[[keywords|Keyword]]: `type`

One of the [[primitive judgment|primitive judgments]].

Introduces a new type.

Types may be interpreted or uninterpreted.

An uninterpreted type is treated as a set with at least one element, but no _specific_ elements.

An interpreted type has specific elements, defined either through [[theory instantiation]] or by enumerating the elements of the type "in extension".

## Examples:

### Example: uninterpreted type
```
type t
```

This judgment can be read as "let `t` be a type". It is admissible provided `t` is a new symbol that has not been used up to this point in the development.

### Example: interpreted type defined by enumeration

```
type color = {red,green,blue}
```

This declares a type with exactly three distinct values, and also declares individuals `red`, `green` and `blue` as its elements.

### Example: interpreted type defined by theory instantiation

```
type foo
interpret foo -> int
```

This declares a type `foo` with an interpretation that is a separate copy of the theory of integers (including overloaded symbols for most integer operators such as `+` and `<`)
[[keywords|Keywords]]: `variant`, `of`, `isa`

A variant is a way of declaring a `type` that is a subtype of another type.

The syntactic form is `variant <new_type> of <old_type>` for declaring a new uninterpreted type, or `variant <new_type> of <old_type> = <sort>` for an interpreted type (i.e. where `<sort>` is a [[struct]])

A term of a given type can be tested for membership in one of its variants (subtypes) using the `isa` operator.

## Example

```
type num
type msg

variant point_msg of msg = struct {
    x: num,
	y: num
}

var saw_message: bool

action foo = {
    var a:point_msg;
    var b:msg := a;
	if b isa point_msg {
        saw_message := true
    }
}
```

In this example, a type `msg` is introduced and then a subtype `point_msg` is defined with an interpretation as a `struct` with two fields. In the action `foo`, a value of the subtype `point_msg` is assigned to a variable of the supertype `msg`.
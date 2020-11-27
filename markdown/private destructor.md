[[keywords|Keyword]]: `destructor`

Typically when a [[type]] is declared as having a specific [[struct]] definition, a set of implicit "destructor functions" are declared to extract the fields of it.

A type may have its fields hidden instead, using a **private destructor** function (declared with `destructor`) that provides access to its fields only for code that can see the private destructor.

## Example:

```
type num 

implementation {
    type integer
	interpret integer -> int
	destructor repr(N:num) : integer
}
```

In this example, code that can see the implementation will see the type `num` as interpreted as the type:

```
struct num {
    repr: integer
}
```

Outside the implementation, the type `num` will be uninterpreted.
[[keywords|Keyword]]: `struct`

A `struct` is an aggregate of named components. Structs are declared like this:

```
type point = struct {
    x : coord,
    y : coord
}
```

This defines a [[type]] `point` whose elements are structures consisting of members `x` and `y` of type `coord`.

Two *destructor* functions `x` and `y` are defined that respectively extract the `x` and `y` coordinates of the point. 

Thus, if `p` is a point, then `x(p)` is the `x` coordinate of `p` and `y(p)` is the `y` coordinate.

The members of a structure can be assigned. For example, to assign `0` to the `x` coordinate of point `p`, we would write:

```
x(p) := 0
```

This mutates not the destructor function `x`, which is immutable, but rather the value of `p`. The value of `y(p)` remains unchanged by this [[assignment]].

Members of a struct can also be [[function|functions]] and [[relation|relations]]. For example:

```
type point = struct {
    coords(X:dimension) : nat
}
```

Our points now have one coordinate value for each dimension defined by some arbitrary type `dimension`. Suppose we declare:

```
type dimension = {x,y,z}
```

We could then refer to the `x` coordinate of a point `p` as `coord(p,x)`, and we could assign `0` to it like this:

```
coord(p,x) := 0
```

This would be written in many programming languages as something like `p.coord[x] = 0`. The type `dimension` need not be an enumerated type, however. It could be an [[uninterpreted type]], or an [[theory instantiation|interpreted type]] (for example, an integer type) or even another structure type.

Structs can also contain structs. For example:

```
type line = {
    begin : point,
    end : point
}
```

To set the `x` coordinate of the begin point of a line `l`, we could write:

```
coord(begin(l),x) := 0
```

This mutates `l` and not the immutable destructor functions `coord` and `begin`.

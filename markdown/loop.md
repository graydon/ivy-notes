[[keywords|Keyword]]: `while`

Loops are discouraged in Ivy. Often, the effect of a loop can be described using an assignment or an `if some` conditional. 

For example, instead of something like this (note: IVy has no `for` loops at all):

```
# Note: not legal IVy code
for y in type {
    link(x,y) := false
}
```

we can write this, using a [[free variable]] in an [[assignment]]:

```
link(x,Y) := false
```

When a loop is needed, it can be written like this:

```
sum := 0;
i := x;
while i > 0
{
    sum := sum + f(i);
    i := i - 1
}
```

This loop computes the sum of `f(i)` for `i` in the range `(0,x]`.

## Loop invariants

A loop can be decorated with a [[invariant|invariants]], like this:

```
while i > 0
invariant sum >= 0
{
    sum := sum + f(i);
    i := i - 1
}
```

The invariant `sum >= 0` is a special assertion that is applied on each loop iteration, before the evaluation of the condition. Invariants are helpful in proving properties of programs with loops.

## Termination and ranking functions

In some situations we need to guarantee that a loop always terminates. We can do this with ranking function that is supplied by the keyword `decreases`, like this:

```
while i > 0
invariant sum >= 0
decreases i
{
    sum := sum + f(i);
    i := i - 1
}
```

The argument of `decreases` is an expression whose value must decrease with every loop iteration and such that the loop is never entered when the expression is less than `0`.

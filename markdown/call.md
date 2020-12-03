[[keywords|Keyword]]: `call`

An [[action]] can call another action, passing arguments, using the `call` keyword, for example:

```
action connect_unique(a:node, b:node) = {
    call clear(a);
    call connect(a,b)
}
```
    
Ivy uses the [call-by-value](https://en.wikipedia.org/wiki/Evaluation_strategy#Call_by_value) convention. That is, when we call `clear(a)` a local variable `x` is created during the execution of `clear` and assigned the value `a`. This means that, as in the [C programming language](https://en.wikipedia.org/wiki/C_(programming_language)), modifying the value of `x` in `clear` would not result in modifying the value of `a` in `connect_unique`.

The return values of an action can be obtained like this:

```
call x,y := swap(x,y)
```

An action with a single return value can be called within an [[expression]]. For example, if `sqrt` is an action, then:

```
x := y + sqrt(z)
```

is equivalent to

```
call temp := sqrt(z)
x := y + temp
```

If there is more than one call within an [[expression]], the calls are executed in left-to-right order. Calls inside [[conditional]] operators occur whether or not the condition is true. For example, the [[statement]]:

```
x := sqrt(y) if c else z
```
    
is equivalent to:

```
call temp := sqrt(y);
x := temp if c else z
```

Parentheses are not used when calling an action with no parameters. For example, if we have:

```
action next returns (val:t) = {
    current := current + 1;
    val := current
}
```

then we would write:

```
x := y + next
```

The lack of parentheses introduces no ambiguity, since the action `next` is not a value and cannot itself be passed as an argument to the function `+`. An advantage of this convention is that we don't have to remember whether `next` is an [[action]] or a [[declared variable]], and we can easily replace a variable by an action without modifying all references to the variable.

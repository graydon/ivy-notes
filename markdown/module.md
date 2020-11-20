keyword: `module`

A module in IVy is a group of declarations that can be [[module instantiation|instantiated]].

In this way it is similar to a template class in an object-oriented programming language.

Any IVy [[declaration]] can be contained in a module.

An instance of a module is called an [[object]], and there is special syntax for declaring a [[singleton object]] that is the sole instance of a module. A further special type of object is an [[isolate]], which is verified in isolation from other objects.

Besides defining classes of objects, modules can be used to capture a re-usable theory, or structure a modular proof.

Modules can contain `private` blocks, which contain declarations not visible outside the module. These are typically used to contain invariants that rule out unreachable states, supporting a publicly visible inductive invariant.

A module can also use `specification` and `implementation` blocks to split its declarations between abstract logical specifications and concrete implementations. Examples of this are shown in the page on [[isolate|isolates]].

## Examples:

## Example: counter
Here is a simple example of a module representing an up/down counter
with a test for zero:

```
module counter(t) = {
    individual val : t

    after init {
        val := 0
    }

    action up = {
        val := val + 1
    }

    action down = {
        val := val - 1
    }

    action is_zero returns(z : bool) = {
        z := (val = 0)
    }
}
```

This module takes a single parameter `t` which is the type of the counter value `val`.

### Example: a theory of partial order

 As an example, here is a module representing a theory of partial orders:

```
module po(t,lt) = {
    axiom lt(X:t,Y) & lt(Y,Z) -> lt(X,Z)
    axiom ~(lt(X:t,Y) & lt(Y,X))
}
```

This module takes a type `t` and a relation `lt` over `t`. It provides axioms stating that `lt` is transitive and antisymmetric.
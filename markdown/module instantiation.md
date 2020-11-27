Keyword: `instance` and/or `instantiate` (the keywords are synonyms)

[[module|Modules]] can be instantiated into [[object|objects]], similar to the way template classes in programming languages can be instantiated.

(Module instantiation should not be confused with the [[instantiation|many other concepts named "instantiation"]] in IVy).

Instantiating a [[module]] creates a separate copy of the [[declaration|declarations]] of the module with any supplied arguments substituted for the module's parameters.

Instantiation may take one of two forms:
  - Creating a freshly-named object, typically with the `instance` keyword (see the first example below).
  - Instantiating a module's declarations into the enclosing object, typically with the `instantiate` keyword (see the second example below).

In cases with many instances with many parameters, it may be more convenient to use an [[autoinstance]] declaration.

## Examples:

### Example: named instantiation of counter module

Given a [[module]] declaration such as: 

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

we can create an instance of the module like this:

```
     type foo
     instance c : counter(foo)
```

This creates an *[[object]]* `c` with members `c.val`, `c.up`, `c.down` and `c.is_zero`. The individual member `c.val` is of type `foo`.

### Example: anonymous instantiation of partial order module

For example, given this module (a theory of partial orders):

```
    module po(t,lt) = {
        axiom lt(X:t,Y) & lt(Y,Z) -> lt(X,Z)
        axiom ~(lt(X:t,Y) & lt(Y,X))
    }
```

We can instantiate the theory into the current object (or at the limit, the global object) like this:

```
    type foo
    instantiate po(foo,<)
```

Notice that we passed the overloaded infix symbol `<` as a parameter. Any symbol representing a [[type]], [[function]], [[relation]], [[action]] or [[object]] can be passed as a [[module]] parameter.
[[keywords|Keywords]]:  `mixin`, `execute`, `before`, `after`, `around`

A mixin is a (possibly anonymous) [[action]] whose execution is coupled to some other named target action (possibly in a different [[module]] or [[object]]) such that any call to the target action is preceded or followed by a call to the mixin.

IVy also often refers to a mixin as a "[[monitor]]", and as an object that only contains mixins for some other object as a "monitor object".

Each mixin is declared with a mode:
  - `mixin X before Y` executes mixin action `X`  before target action `Y`
  - `mixin X after Y` executes mixin action `X` after target action `Y`
	
Mixins are invoked with the same arguments as their target actions. An `after` mixin can also see the (named) return value of the target action.

Any [[precondition]] or [[postcondition]] of a mixin is added to the target action as a [[precondition]] or [[postcondition]] of the target.

Mixins may be declared with separate [[visibility qualifiers]] from implementations. It is typical to declare mixins with pre and post conditions for an action that is otherwise abstract in a `specification` block, while placing the body of the action in the `implementation` block.

Mixins are ubiquitous enough that they have several abbreviations and synonyms:

  - The keyword `execute` is a synonym for `mixin`, so `execute X before Y` is a mixin declaration identical to `mixin X before Y`
  - Actions that exist *solely* to be mixins may be declared anonymously and associated with a target in a single declaration:
    - `before Y(<arguments>) { <statements> }`
    - `after Y(<arguments>) { <statements> }`
  - Since the arguments for an mixin must match those of their target action, the arguments list of an anonymous mixin may be omitted -- it will be implicitly identical to that of the target, with the same argument names:
	  - `before Y { <statements> }`
	  - `after Y { <statements> }`
  - As a further abbreviation, a pair of anonymous before/after mixins may be declared using `around X { <before-statements> ... <after-statements> }` where `...` is a literal punctuation token consisting of three dots.
 
 ## Examples
 
 ## Example: adding a precondition
```
action pre_connect(x:client,y:server) = {
    require semaphore(y)
}        
execute pre_connect before connect
```
 
 This example adds a [[precondition]] to any call to some [[action]] `connect(x:client,y:server)`. It is equivalent to declaring:
 
```
before connect(x:client,y:server) {
    require semaphore(y)
}
```

Or even more tersely:

```
before connect {
    require semaphore(y)
}
```

## Example: adding a postcondition
```
action post_incr(inp:t) returns(out:t) = {
    ensure inp < out
}

execute post_incr after incr
```

This example adds a [[postcondition]] to any call to some [[action]] `incr(inp:t) returns (out:t)`. It is equivalent to declaring:

```
after incr(inp:t) returns(out:t) {
    ensure inp < out
}
```

Or even more tersely:

```
after incr {
    ensure inp < out
}
```

[[keywords|Keyword]]: `proof`

A proof is a sequence of [[tactic]] invocations that either names or trails a [[logical judgment]] establshing its validity.

Multiple tactic invocations may optionally be separated by a semicolon (`;`) and the entire proof may be (and typically is) enclosed in curly braces (`{...}`)

If a proof is not supplied for a judgment, IVy's default [[tactic]] will attempt to prove validity automatically.

## Examples:

### Example: proof trailing a judgment

```
include deduction

theorem [symm] {
    type t
    property X:t = Y
    property Y:t = X
}
proof { 
    apply introEq;
    apply elimEq with x = X, y = Y
}
```

This proof follows the [[theorem]] named `[symm]` and consists of two `apply` tactics. 

The first tactic applies the labeled [[axiom schema]] `introEq`  from IVy's `deduction` standard header, allowing IVy to unify the premises of that axiom schema with the proof goal.

The second applies the `elimEq` axiom schema (also from the `deduction` header) and explicitly instantiates the premises `x` and `y` of that axiom schema with the goal variables `X` and `Y`

### Example: separate proof declaration

```
include deduction

theorem [symm] {
    type t
    property X:t = Y
    property Y:t = X
}

type j
individual k:j

# ... sometime later
proof [symm] {
    apply introEq;
    apply elimEq with x = X, y = Y
}
```

This proof is identical to the first example, but shows that a proof can be given for a labeled judgment in some significantly-later declaration, by referring to the label of the judgment.
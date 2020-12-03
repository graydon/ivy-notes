A free variable is a [[logical variable]] that occurs outside of any [[quantifier]].

Free variables in Ivy are **implicitly** [[universal quantifier|universally quantified]].

In the case of a [[declaration]], this is equivalent to writing the universal quantifiers in the declared formula explicitly.

In the case of a [[statement]], this is equivalent to an implicit [[loop]] over all possible values of the variable.

## Examples:

### Declaration
The following two declarations are equivalent

```
# Explicit quantifiers:
axiom [foo] forall X:t. forall Y:t. bar(X,Y)

# Implicit quantifiers
axiom [foo] bar(X,Y)
```

### Statement

In the following example, the two groups of assignments are equivalent

```
type t = {red, green}
relation foo(X:t, Y:t)

action bar = {

    # Explicit assignments
	foo(red, red) := true
	foo(red, green) := true
	foo(green, red) := true
	foo(green, green) := true
	
	# Implicit assignment, ranging over all X:t and Y:t
	foo(X, Y) := true
	
}
```
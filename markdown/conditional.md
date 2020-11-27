[[keywords|Keywords]]: `if`, `else`

Conditionals in IVy are [[statement|statements]] that behave much as in any procedural programming language.

Special [[nondeterminism|nondeterministic]] forms also exist:
  - [[nondeterministic conditional]]
  - [[nondeterministic choice]]


## Examples:

For example, the following code clears the incoming links to node `y` if `y` is in the failed set:

```
if failed(y) {
    link(X,y) := false
}
```

The curly brackets around the assignment are required. No parentheses are need around the condition.  A conditional can have an associated `else` clause, for example:

```
if failed(y) {
    link(X,y) := false
} else {
    link(y,z) := true
}
```

Because brackets are required, there is no ambiguity as to which `if` an `else` belongs to.

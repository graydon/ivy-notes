[[declaration|Declarations]] in a module (and therefore also an [[object]] or [[isolate]]) can be qualified by one of three different levels of visibility, similar to the visibility qualifiers in a conventional programmaing language:

  - **specification** declarations, in a `specification { ... }` block
  - **implementation** declarations, in an `implementation { ... }` block 
  - **private** declarations, in a `private { ... }` block

These blocks may occur (syntactically) within [[modules]], [[objects]], or in-line [[isolate]] declarations. That is, one can write any of the following:

  - `module foo = { ... specification { ... } }`
  - `object foo = { ... specification { ... } }`
  - `isolate foo = { ... specification { ... } }`

Each of these blocks marks all the declarations inside it as having the given visibility, which in turn affects how verification treats them and, in particular, the **relationships** between the visibility levels of declarations in the current isolate and those in any other isolate that's provided as a `with` argument to the current isolate declaration. The visibility-modifying blocks do not otherwise affect the module or object nesting structure. 

## Meaning of visibility qualifiers
Private declarations are not visible outside the module. These are typically used to contain invariants that rule out unreachable states, supporting a publicly visible inductive [[invariant]].

Specification and implementation declarations allow splitting declarations in two pieces.

  - The specification declarations in an [[object]] `X` are visible to any other isolate that is declared `isolate { ... } with X`. That is, when `X` is supplied as a visible input to the other isolate.
  - The implementation declarations in an object are verified when the object itself is verified, and are checked for consistency with any specification they are a part of.

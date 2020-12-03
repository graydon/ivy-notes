Expressions -- also called [[term|terms]] or [[formula|formulas]] -- in Ivy are drawn from the many-[[sort|sorted]] theory of [first-order logic](https://en.wikipedia.org/wiki/First-order_logic) with equality.

Ivy provides the following built-in logical operators:
  - `~` (not)
  - `&` (and)
  - `|` (or),
  - `->` (implies)
  - `<->` (iff)
  - `=` (equality)

In addition to several other arithmetic and relational operators that are overloaded and available on certain [[type|types]] if they are [[theory instantiation|interpreted]] as a numerical [[sort]].

There is also a built-in conditional operator `X if C else Y` that returns `X` if the Boolean condition `C` is true and `Y` otherwise.

The if/else operator binds most strongly, followed by equality, not, and, or. The weakest binding operators are `<->` and `->`, which have equal precedence.

The binary and ternary operators are left-associating (i.e., they bind more strongly on the left). For example, `x if p else y if q else z` is equivalent to `(x if p else y) if q else z` and `p -> q -> r` is equivalent to `(p -> q) -> r`. **Warning**: in the case of if/else and `->`, this is non-standard and is due to an error in the parser. This will change in a future version of the language. In the interim it is best to always parenthesize expressions with multiple uses if if/else
and `->`.

Expressions may also use logical quantifiers. For example, this formula says that
there exists a node `X` such that for every node `Y`, `X` is linked to `Y`:

```
exists X. forall Y. link(X,Y)
```

In this case, the variables do not need type annotations, since we can infer that
both `X` and `Y` are nodes. However, in some cases, annotations are needed. 

For example, this is a statement of the transitivity of equality:

```
forall X,Y,Z. X=Y & Y=Z -> X=Y
```

We can determine from this expression that `X`, `Y` and `Z` must all be of the same type, but not what that type is. This means we have to annotate at least one variable, like this:

```
forall X:node,Y,Z. X=Y & Y=Z -> X=Y
```

## Grammar

A condensed grammar follows, in which any pluralized production name (eg, `<terms>` or `<vars>`) is a comma-separated list of the singular production name.

`<term>` is any of:
  - `<term> <binop> <term>` where `<binop>` is any of `*`, `+`, `/`, `-`
  - `<term> <relop> <term>` where `<relop>` is any of `=`, `~=`, `<`, `<=`, `>=`, `>`, `*>`
  - `<term> <logop> <term>` where `<logop>` is any of `&`, `|`, `->`, `<->`
  - `~ <term>`
  - `true` or `false`
  - `forall <simplevars> . <term>`
  - `forall (<vars>) <term>`
  - `exists <simplevars> . <term>`
  - `exists (<vars>) <term>`
  - `globally <term>`
  - `eventually <term>`
  - `<term> isa <atype>`
  - `<symbol>(<terms>)`

`<simplevar>` is any of:
  - `<variable>`
  - `<variable> : <symbol>`

`<var>` is any of:
  - `<variable>`
  - `<variable> : <atype>`

`<atype>` is any of:
  - `<symbol>`
  - `<atype> . <symbol>`
  - `this`

`<variable>` and `<symbol>` are the corresponding [[lexical structure|lexical tokens]]

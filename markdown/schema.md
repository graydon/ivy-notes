[[keywords|Keywords]]: `schema`, `axiom`, `theorem`

A compound judgment or "schema" takes a collection of judgments as input (the premises) and produces a judgment as output (the conclusion).

The keyword `schema` introduces an [[axiom schema]] as does the block form of an `axiom` declaration. The keyword `theorem` introduces a [[theorem]].

If the schema is valid, then we can provide any type-correct values for the premises and the conclusion will follow.

The default [[proof]] [[tactic]] never uses a schema automatically; to use a schema, it must be explicitly [[judgment application|applied]]. This allows us to express and use facts that, if they occurred in verification conditions, would take us outside the [[logical fragment|decidable fragment]].

## Example:

```
axiom [congruence] {
    type d
    type r
    function f(X:d) : r
    #--------------------------
    property X = Y -> f(X) = f(Y)
}
```

The schema is contained in curly brackets. This differentiates it from a [[primitive judgment]].

Within the curly brackets there is a list of premises, followed by a conclusion.

The dashes separating the premises from the conclusion are just a comment.

The conclusion is always the last judgment in the schema.

In this case, it says that, given types `d` and `r` and a function `f` from `d` to `r` and any values `X`,`Y` of type `d`, we can infer that `X`=`Y` implies `f(X) = f(Y)`.

Also, notice the declaration of function `f` contains a variable `X`. The scope of this
variable is only the function declaration. It has no relation to the variable `X` in the conclusion.

The keyword `axiom` tells Ivy that this schema should be taken as valid without proof: it is an [[axiom schema]] rather than a [[theorem]].
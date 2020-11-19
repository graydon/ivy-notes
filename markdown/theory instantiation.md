[[keywords|keyword]]: `interpret`

One of the [[primitive judgment|primitive judgments]].

(Not to be confused with the [[instantiation|many other meanings of the term "instantiation"]])

The normal way of using IVy is to declare uninterpreted [[type|types]] and to give the necessary [[axioms]] over those types to prove desired [[property|properties]] of a system. However, it is also possible in IVy to associate [[type|types]] with sorts that are interpreted in the underlying theorem prover by declaring `interpret <ty> -> sort`

Concrete sorts that are currently available for interpreting IVy types are:

- `int`: the integers
- `nat`: the non-negative integers
- `{X..Y}`: the subrange of integers from `X` to `Y` inclusive
- `{a,b,c}`: an enumerated type
- `bv[N]`: bit vectors of length `N`, where `N > 0`

Arithmetic on `nat` is **saturating**. That is, any operation that would yield a neagtive number instead gives zero. 

An arbitrary function or relation symbol can be interpreted. This is useful for symbols of the theory that have no pre-defined overloaded symbol in IVy.

## Examples:

### Example: integers

    type idx
    interpret idx -> int

This says that IVy type `idx` should be interpreted using sort `int` of the theorem prover. This does not mean that `idx` is equated with the integers. If we also interpret type `num` with `int`, we still cannot compare values of type `idx` and type `num`. In effect, these two types are treated as distinct copies of the integers.

When we declare `idx` as `int`, certain overloaded [[function|functions]] and [[relation|relations]] on `idx` are also automatically interpreted by the corresponding operators on integers, as are numerals of that type. So, for example, `+` is interpreted as addition and `<` as "less than" in the theory of integers. Numerals are given their normal interpretations in the theory, so `0:idx = 1:idx` would be false.

### Example: bit vectors

```
type t
type s
function extract_lo(X:t) : s
    
interpret t -> bv[8]
interpret s -> bv[4]
interpret extract_lo -> bfe[3][0]
```

Here `bfe[3][0]` is the bit field extraction operator the takes the low order 4 bits of a bit vector.

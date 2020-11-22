"Finite almost uninterpreted" (FAU) is a [[logical fragment]] of [[first-order logic]] that extends [[FEU]] with one (important) special case: whereas [[universal quantifier|universal]] [[logical variable|variables]] are prohibited from use as arguments to [[theory instantiation|interpreted]] functions in FEU, in FAU they are allowed to be arguments to [[arithmetic literal|arithmetic literals]].

## Example

For example, the following formula is in FAU but not FEU:

```
forall X. 0 <= X & X < m - 1 -> p(X)
```

Arithmetic literals can be useful, for example, when reasoning about the contents of an array indexed by the integers. On the other hand, in IVy it is more typical to treat array indices as an uninterpreted totally ordered type.
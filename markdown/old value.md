[[keywords|Keyword]]:  `old`

In logical statements within an action -- such as [[assertion|assertions]] or [[loop]] invariants -- an [[expression]] `old <expr>` refers to the values of the [[state variable|state variables]] in `<expr>` before the [[transition relation]] that the enclosing action or statement models.

## Examples:

### Example: old state variable in postcondition

Suppose we have a function `reverse2` that reverses a [[state variable]] `x` twice, leaving it equal to the value it started with. Then the following [[mixin]] can be written that uses `old` to refer to the old value along with the new one:

```
specification {
    after reverse2 {
        ensure x = old x
    }
}
implementation {
    implement reverse2 {
        x := my_rev.reverse(x);
        x := my_rev.reverse(x);
    }
}
```

### Example: old state variable in loop invariant

The following example implements array reversal in terms of a `while` [[loop]]. The loop invariant refers to the array's state at each iteration of the loop and relates it to the `old` state of the array, in the previous iteration (or the state on-entry to the loop).

```
    implement reverse {
        if a.end > 0 {
            var i : idx := 0;
            var j := rt.reverse(i,a.end);
            while i < j
            invariant 0 <= i & i <= a.end & a.end = old a.end & rt.rev(a.end,i,j)
            invariant forall I. (0 <= I & I < i | j < I & I < a.end) 
                                & rt.rev(a.end,I,J)-> a.value(J) = old a.value(I)
            invariant i <= I & I <= j -> a.value(I) = old a.value(I)
            {
                var tmp := a.value(j);
                a := a.set(j,a.value(i));
                a := a.set(i,tmp);
                i := i.next;
                j := rt.reverse(i,a.end);
            };
        }
    }
```

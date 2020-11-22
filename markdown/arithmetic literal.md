An **arithmetic literal** is a [[formula]] of the form `X < Y`, `X < t`, `t < X` or `X = t` where `X` and `Y` are universal variables and `t` is a [[ground term]], all of integer type.

We allow only arithmetic literals that occur [[positive and negative occurrences|positively]].  However, a negative occurrence of `x < y` can be converted to `~(x = y | y < x)`, while a negative occurrence of `x = t` can be eliminated by a method called "destructive equality resolution".

For arithmetic literals, we add the following rule to the construction of the [[relevant vocabulary]] graph:

| positive term          | arc(s)              |
|------------------------|---------------------|
| `X < Y`                  | `V[X] <-> V[Y]`       |

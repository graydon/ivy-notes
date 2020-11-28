[[keywords|Keyword]]: `autoinstance`

The [[declaration]]

```
autoinstance x[p1][p2] : y(p1...pn)
```

causes an [[module instantiation|instance]] `x[p1][p2]` of `y(p1...pn)` to be generated for each identifier `x[p1][p2]` that occurs undefined in the program, for any values of `p1`...`pn`.

The [[module]] `y` is instantiated just before the [[declaration]] triggering the instantiation. 

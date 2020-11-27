[[keywords|Keyword]]: `alias`

An alias is a synonym for some existing declaration. It is typically used to shorten or clarify code.

## Example:

```
module set(elem) = {

    type this
    alias t = this

    relation contains(X:t,Y:elem)
    action emptyset returns(s:t)
    action add(s:t,e:elem) returns (s:t)
    ...
}
```

Notice something new here: `type this`.  This declares a [[type]] with the same name as the [[object]] we are declaring (which won't be known until we [[module instantiation|instantiate]] this [[module]]).

For convenience, we create an alias `t` for this type. This interface of our abstact set type contains a relation and two actions.

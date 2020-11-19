[[keywords|Keyword]]: `object`

Abbreviation of a [[module]] declaration and a simultaneous single [[module instantiation|instantiation]] of it.

## Example:

We can create a module with just one instance like this:

```
object foo = {
    relation bit
    after init {
        bit := false
    }
    action flip = {
        bit := ~bit
    }
}
```

This creates a single object `foo` with members `foo.bit` and `foo.flip`, exactly as if we had created a module and instantiated it once.

An [[isolate]] is a special kind of `object`.
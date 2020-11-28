[[keywords|Keyword]]: `thunk`

A thunk is a first-class executable sequence of [[statement|statements]] declared within an [[action]].

It is similar to a closure in a functional programming language.

## Example

```
type callback
action run_callback(f:callback)

var s:bool
action foo = {
    thunk [some_label] set_s : callback := {
        s := true
    };
    call run_callback(set_s)
}
```
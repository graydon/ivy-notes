[[keywords|Keyword]]: `action`

A type of [[declaration]].

Actions have a declaration and an implementation. These may be separated -- for example by declaring the action in an [[isolate]]'s interface and providing the action's implementation in the isolate's implementation block -- or the action's declaration may be followed in-line by an equals sign (`=`) and an implementation block.

Actions are similar to procedures in a typical imperative programming language, containing [[statements]] which in turn evaluate [[expressions]] and modify [[state variable]].

Actions are semantically synchronous. This means that:

  - All actions occur in reaction to input from the environment, and
  - All actions are isolated, that is, they appear to occur instantaneously, with no interruption.

Actions may or may not terminate. An action terminates when control reaches the end of the action without any statements failing. If any statement fails, the action does not terminate.

An action is logically interpreted as a [[transition relation]] over [[state variable|state variables]].

## Examples:

### Example: assigning a state variable

```
action connect(x:node, y:node) = {
    link(x,y) := true
}
```




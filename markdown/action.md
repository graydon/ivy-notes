[[keywords|Keyword]]: `action`, `returns`

A type of [[declaration]].

Actions have a declaration and an implementation. These may be separated -- for example by declaring the action in an [[isolate]]'s interface and providing the action's implementation in the isolate's `implementation` block -- or the action's declaration may be followed in-line by an equals sign (`=`) and an implementation block.

Actions are similar to procedures in a typical imperative programming language, containing [[statement|statements]] which in turn evaluate (and possibly statically verify) [[expression|expressions]] and modify [[state variable]].

Actions are semantically synchronous. This means that:

  - All actions occur in reaction to input from the environment, and
  - All actions are isolated, that is, they appear to occur instantaneously, with no interruption.

An action may return a value by listing a `returns (<symbol>:<type>)` in its signature. The named return value must then be assigned within the action.

An action is logically interpreted as a [[transition relation]] over [[state variable|state variables]].

## Examples:

### Example: assigning a state variable

```
action connect(x:node, y:node) = {
    link(x,y) := true
}
```

### Example: separate implementation

```
isolate network = {
    type node
	relation link(X:node, Y:node)
	
    specification {
	   action connect(x:node, y:node)
	}
	
	implementation {
	    implement connect {
		    link(x,y) := true
		}
	}
}
```

### Example: returning a value

```
type t
interpret t -> int

action incr(x:t) returns (out:t) = {
   out := x + 1
}
```
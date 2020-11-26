[[keywords|Keyword]]: `import`

An **imported action** is an action whose implementation is provided by the environment.

Imported actions are declared like any other action. A separate `import <action>` declaration marks them as imported.

An imported action may not be implemented by the program, but it can be given [[mixin|mixins]].

## Example:

For example:

```
action callback(x:nat) returns (y:nat)
import callback
```

or simply:

```
import action callback(x:nat) returns (y:nat)
```

Like any action, the imported action may be given preconditions and postconditions.  For example:

```
before callback {
    require x > 0;
}
    
after callback {
    ensure y = x - 1;
}
```
    
The `require` in this case is guarantee for the program and an assumption for the environment. Similarly, the `ensure` is an assumption for the program and a guarantee for the environment.  The environment is assumed to be non-interfering, that is, Ivy assumes that the call to `callback` has no visible side effect on the program. 

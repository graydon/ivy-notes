Ivy provides a few different commands to work with `.ivy` files:

  - `ivy_show` prints the elaborated form of a [[program]] and exits
  - [[ivy_check command|ivy_check]] performs [[verification condition|verification]]
  - [[ivy_to_cpp command|ivy_to_cpp]] performs [[code extraction]]
  - [[ivy command|ivy]] runs a GUI for exploring the evolution of a program's [[state variable|state variables]]

These commands should be available in your `$PATH` after installation. If not, installation was probably unsuccessful.

Ivy's commands share a common argument syntax:

```
*command* *option*=*value* ... *file*.ivy
```

## Common options
All commands take some common options. In the following, `name` represents a hierarchical Ivy name, which may contain the `.` character, and `boolean` represents a Boolean value `true` or `false`.

`isolate=name`

This options specifies the name of a [[isolate]] to verify or extract. 

`show_compiled=boolean`

If `true`, this option causes a representation of the elaborated Ivy code to be printed before doing any other work. The elaborated code reflects all the [[module instantiation|module instantiations]] as well as the various transformations performed by Ivy's compiler to produce an [[isolate]]. This is useful to see in detail what is contained in an isolate.

`pedantic=boolean`

If `true`, certain optional warnings are enabled. The default value is `false`.

[[keywords|Keyword]]: `parameter`

A **parameter** is a value supplied by the [[environment]] before [[initializer|initialization]].

A parameter is [[declaration|declared]] like this:

```
parameter p : t
```

where `p` is the parameter name and `t` is the type.

Parameters may be declared anywhere in the object hierarchy.

Except for the fact that it is initialized by the environment, a parameter is identical to an [[individual]].

The manner in which parameters are supplied is dependent on the compiler. For example, if a program is compiled to an executable file, the parameter values are supplied on the command line. If it is compiled to a class in C++, parameters are supplied as arguments to the constructor.

In either case, the order of parameters is the same as their order of declaration in the program.

[[keywords|Keyword]]: `extract`

**code extraction** refers to translating IVy source to some other language. The standard code extraction [[command]] is [[ivy_to_cpp command|ivy_to_cpp]]

The `ivy_to_cpp` command can extract any specified [[isolate]] but it is common to declare an `extract` that bundles together a set of isolates and, optionally, introduces one or more declaration parameters. The extracted code is then considered as an instance at a single (arbitrarily chosen) element of the parameter type.

## Example:

For example, if we have some type `node` and some declarations for a module `leader(n:node)` and parameter `pid(n:node)`. Then we can specify an extract named `code` at a single arbitrary representative `node` value like so:

```
extract code(n:node) = leader(n), pid(n), node, id
```

The extract is parameterized on a node `n`. It consists of the `leader` isolate for parameter `n`, the value of the `pid` map for `n`, as well as the node and id isolates (which are not parameterized, and which therefore cannot have state). 

When we extract code, the [[visibility qualifiers|specification]] sections of the [[isolate|isolates]] are erased.  The specification [[state variable|state variables]] and code (including assertions) are not present in the extract. IVy verifies that this erasure is sound, in the sense that the erased code has no effects that are visible to the remaining code (including non termination). This means that the observable behavior of the program is not affected by erasure of the specifications.
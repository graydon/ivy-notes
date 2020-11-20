[[keywords|Keyword]]: `isolate`

An isolate is an isolated context for verification.

An isolate is also an [[object]], meaning that is an instance of a [[module]]. Declaring an isolate therefore requires either [[declaration|declaring]] and defining an object in-line, or else declaring that an existing object is an isolate (i.e. that it should be verified in isolation).

The isolation of an isolate is accomplished through information-hiding similar to visibility control in a programming language. Specifically, declarations in an isolate are treated differently according to 3 different levels of visibility:

  - **specification** declarations, in a `specification { ... }` block
  - **implementation** declarations, in an `implementation { ... }` block 
  - **private** declarations, in a `private { ... }` block

These blocks may occur within modules, objects, or in-line isolate declarations. That is, one can write any of the following:

  - `module foo = { ... specification { ... } }`
  - `object foo = { ... specification { ... } }`
  - `isolate foo = { ... specification { ... } }`

Each of these blocks marks all the declarations inside it as having the given visibility, which in turn affects how verification treats them and, in particular, the **relationships** between the visibility levels of declarations in the current isolate and those in any other isolate that's provided as a `with` argument to the current isolate declaration. The visibility-modifying blocks do not otherwise affect the module or object nesting structure. 

By default, every program has a single top-level isolate (which is anonymous but can be denoted by `this` at the top-level). 

Additional isolates can be declared inside the program in a variety of forms:
  - `isolate <symbol> = { ... }` declares and defines an object as an isolate
  - `isolate <symbol> = <symbol>` declares that an existing object should be isolated
  - `isolate <symbol> = <symbol> with <other_isolate>` and
  - `isolate <symbol> = { ... } with <other_isolate>` declares an isolate that includes the specification portion of the isolate `<other_isolate>`

Additional isolates do not automatically have visible access to their enclosing isolate. Instead, they may be given such access by declaring them using `isolate ... with this`

## Example:

```
isolate evens = {

    action step
    action put(n:num)

    specification {
        before put {
            require n.even
        }
    }

    implementation {

        var number : num
        after init {
            number := 0
        }

        implement step {
            call odds.put(number + 1)
        }

        implement put(n:num) {
            number := n;
        }

        invariant number.even
    }
}
with odds,num

isolate odds = {

    action step
    action put(n:num)

    specification {
        before put {
            require n.odd
        }
    }

    implementation {

        var number : num
        after init {
            number := 1
        }

        implement step {
            call evens.put(number + 1)
        }

        implement put {
            number := n;
        }

        invariant number.odd

    }
}
with evens,num
```

This is a pair of isolates that depend on one another. 

Effectively, this breaks the proof that the two assertions always hold into two parts. In the first part, we assume the object `evens` gets correct inputs and prove that it always sends correct outputs to `odds`. In the second part, we assume the object `odds` gets correct inputs and prove that it always sends correct outputs to `evens`.

This argument seems circular on the surface. It isn't, though, because when we prove one of the assertion holds, we are only assuming that the other assertion has always held *in the past*. So what we're really proving is that neither of the two objects is the first to break the rules, and so the rules always hold.

In the first isolate, we prove the assertion that `evens` guarantees. We do this using the visible part of `odds`, but we forget about the hidden state of the `odds` object (in particular, the variable `odss.number`). To model the call to `evens.put` in the hidden part of `odds`, Ivy exports `evens.put` to the environment. The `requires` statement in the specification `even.put` thus becomes a guarantee of the environment. That is, each isolate only guarantees those assertions for which it receives the blame. The rest are assumed.

When we verifiy isolate `evens`, the result is as if we had actually entered the following program:

```
object nat {
    ...
}

object evens = {
    var number : nat
    after init {
        number := 0
    }

    action step = {
        call odds.put(number + 1)
    }

    action put(n:nat) = {
        require even(nat)
        number := n;
    }

    invariant number.even
}

object odds = {

    action put(n:nat) = {
        require odd(nat)
    }
}

export evens.step
export evens.put
```

Notice the implementation of `odds.put` has been eliminated, and what remains is just the assertion that the input value is odd (IVy verifies that the eliminated side effect of `odds.put` is in fact invisible to `evens`). The [[assertion]] that inputs to `evens` are even has in effect become an [[assumption]]. We can prove this isolate is safe by showing that `even.number` is [[invariant|invariantly]] even, which means that `odds.put` is always called with an odd number.

The other isolate, `odds`, looks like this:

```
object nat {
    ...
}

object evens = {
    action put(n:nat) = {
        require even(nat)
    }
}

object odds = {
    var number : nat
    after init {
        number = 1
    }

    action step = {
        call evens.put(number + 1)
    }

    action put(n:nat) = {
        require odd(nat)
        number := n;
    }

    invariant number.odd
}

export odds.step
export odds.put
```

If both of these isolates are safe, then we know that neither assertion is the first to fail, so the original program is safe.

The general rule is that a `require` assertion is a guarantee for the calling isolate and and assumption for the called isolate, while an `ensure` action is a guarantee for the called isolate and an assumption for the calling isolate. When we verify an isolate, we check only those assertions that are gurantees for actions in the isolate.

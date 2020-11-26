Assume-guarantee reasoning is a family of methods for modular verification of concurrent systems.

They are generally related to Jones' "rely-guarantee" reasoning of the early 80s, which in turn is an extension of an earlier Hoare-logic variant for parallel verification developed by Owicki and Gries in the late 70s.

The key concepts in all these approaches are:

  - Abstracting-away portions of the system into **assumptions**.
  - Verifying that the remaining un-abstracted portion of the system upholds **guarantees**.
  - Checking **non-interference** between abstracted and verified portions of the system, to ensure soundness of the abstraction.
  - Checking **coverage** of all assertions in all mixtures of abstraction and verification.

IVy uses [[isolate|isolates]] as its units of assume-guarantee reasoning, along with [[visibility qualifiers]].

Specifically, when verifying some [[isolate]] `X`:

  - The [[action|actions]] and [[state variable|state variables]] of `X` are preserved and verified, as well as all actions and state variables at the `specification` [[visibility qualifiers|visibility level]] of any other isolates listed in the `with` portion of `X`.
  - The contents of all other isolates in the program -- including `implementation` and `private` [[visibility qualifiers|visibility levels]] of isolates listed in the `with` clause of `X` -- are abstracted (erased) from verification.
  - Any action that is to be abstracted is first checked to ensure that it does not modify any of the same state variables as any of the actions in `X`. If it does, an error is signalled.
  - All [[guarantee|guarantees]] in the system are checked for coverage from the perspective of each [[isolate]].

Furthermore, all [[precondition|preconditions]] and [[postcondition|postconditions]] are contextually interpreted as [[assumption|assumptions]] and [[guarantee|guarantees]], depending on caller/callee relationship to the isolate `X`.

Specifically:

  - The following are [[guarantee|guarantees]] in `X` (that verification must prove):
    - A `require` assertion in another isolate that `X` calls into
    - An `ensure` assertion within `X`

- The following are [[assumption|assumptions]] in `X` (that verification may assume):
    - A `require` assertion within `X`
    - An `ensure` assertion in another isolate that `X` calls into


## Example:

Suppose we have the following simple client/server system:
```
isolate client = {
    specification {
        type count
        interpret count -> int
        var n_send:count
        var n_recv:count
        action send_req = {
            n_send := n_send + 1;
            call server.req;
        }
        action recv_res = {
            require n_send > n_recv;
            n_recv := n_recv + 1;
        }
        invariant n_recv <= n_send
    }
    implementation {
        after init {
            n_send := 0;
            n_recv := 0;
        }
    }
} with server

isolate server = {
    specification {
        action req
        before req {
            require client.n_send > client.n_recv;
        }
    }
    implementation {
        implement req {
            call client.recv_res;
        }
    }
} with client

export client.send_req
export client.recv_res
export server.req
```

Each isolate verifies on its own:
  - The client actions establish and maintain the [[invariant]]
  - The client `recv_res` action has a [[precondition]] which is treated as an [[assumption]] guaranteed by the [[environment]] while verifying `client` -- and this allows it to [[guarantee]] the invariant.
  - The [[precondition]] on `recv_res` is met by the `server` requiring it as well in its `specification`

If we begin hiding parts of the implementation, however, we run into a problem. For example if we hide the `implementation` of `send_req` and `recv_res` (while preserving the precondition of `recv_res` in a [[mixin]]):

```
isolate client = {
    specification {
        type count
        interpret count -> int
        var n_send:count
        var n_recv:count
        action send_req
        action recv_res

#+++ New mixin added here
        before recv_res {
            require n_send > n_recv
        }

        invariant n_recv <= n_send
    }
    implementation {
        after init {
            n_send := 0;
            n_recv := 0;
        }

#+++ New implement blocks added here
        implement send_req {
            n_send := n_send + 1;
            call server.req;
        }
        implement recv_res {
            n_recv := n_recv + 1;
        }
    }
} with server

isolate server = {
    specification {
        action req
        before req {
            require client.n_send > client.n_recv;
        }
    }
    implementation {
        implement req {
            call client.recv_res;
        }
    }
} with client

export client.send_req
export client.recv_res
export server.req
```

Now `client` still verifies, but verifying the `server` isolate now fails:

```
Isolate server:
t.ivy: line 34: error: Call out to client.recv_res[implement5] may have visible effect on client.n_recv
t.ivy: line 12: referenced here
t.ivy: line 34: referenced here
```

This happens because IVy tracks which actions modify which state variables, in particular it notices that `client.recv_res` modifies `client.n_recv`.

IVy then attempts to verify `server` in [[isolate|isolation]], and its first step is to check that actions **outside** server can be safely abstracted (erased). Erasing `client.recv_res` is only safe if the variables it alters are only used by other abstract (to-be-erased) actions.

Unfortunately `client.n_recv` is mentioned in the (still-visible, not-erased) precondition in the mixin `before client.recv_res`, and that precondition is a [[guarantee]] that `server` has to maintain. It cannot safely do so if `client.recv_res` is abstracted away, as its effect on `client.n_recv` will be unknown.

If we attempt to recover from this by moving the precondition on `client.recv_res` into the `implement` block as well, as follows:

```
isolate client = {
    specification {
        type count
        interpret count -> int
        var n_send:count
        var n_recv:count
        action send_req
        action recv_res
        invariant n_recv <= n_send
    }
    implementation {
        implement send_req {
            n_send := n_send + 1;
            call server.req;
        }
        implement recv_res {
            require n_recv < n_send;
            n_recv := n_recv + 1;
        }
        after init {
            n_send := 0;
            n_recv := 0;
        }
    }
} with server

isolate server = {
    specification {
        action req
    }
    implementation {
        implement req {
            call client.recv_res;
        }
    }
} with client

export client.send_req
export client.recv_res
export server.req
```

We get a different error:

```
t.ivy: line 19: error: assertion is not checked
t.ivy: line 10: error: ...in action client.recv_res
t.ivy: line 35: error: ...when called from server.req[implement6]
error: Some assertions are not checked
```

This is a coverage error: when verifying `server`, IVy will abstract-away (erase) the implementation of `recv_res`, which means an precondition that is only in the isolate's `implementation` will not be checked.

The best we can do, then, is change the implementation assertion into a dynamic condition, which will at least still preserve the isolate's weaker invariant:

```
isolate client = {
    specification {
        type count
        interpret count -> int
        var n_send:count
        var n_recv:count
        action send_req
        action recv_res
        invariant n_recv <= n_send
    }
    implementation {
        implement send_req {
            n_send := n_send + 1;
            call server.req;
        }
        implement recv_res {
            if n_recv < n_send {
                n_recv := n_recv + 1;
            }
        }
        after init {
            n_send := 0;
            n_recv := 0;
        }
    }
} with server

isolate server = {
    specification {
        action req
    }
    implementation {
        implement req {
            call client.recv_res;
        }
    }
} with client

export client.send_req
export client.recv_res
export server.req
```
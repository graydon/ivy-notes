Preconditions are asserted by the `require` keyword.

Preconditions are _not_ checked at runtime, they are checked statically (logically) by `ivy_check` as it verifies a module's actions, and this makes for counter-intuitive behaviour concerning calls to [[export|exported]] actions made from the environment.

Specifically: placing a precondition on an action `A` does not guarantee that the environment will invoke `A` in a state that meets the precondition, nor that `A` will signal any sort of runtime error should the precondition not hold.

Rather, it adds a _logical assumption_ about the state on entry to `A` when called from the environment. The precondition is simpy assumed to be true. This has to be so, because IVy is not responsible for verifying the environment.

When an action called form another action, preconditions behave more normally (and usefully): the caller is obliged to meet the preconditions of the calee, and the caller's verification fails if it does not.
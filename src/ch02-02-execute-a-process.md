# Execute a process

Bastion is an actor-model-like concurrency. In the `Actor Model`\* we find some `Actor` which are made to process computation. In Bastion we have the `Children`.

A `Children` is spawned by using `Bastion::children();`. It take a `init` closure as parameter, it take the new `Children` as an argument and returning it once configured.

## Exec

The new `Children` passed in the `init` closure as parameter, will be used to configure itself. `with_exec();` sets the closure that will be used by every child of this children group. It take a `BastionContext` as parameter and return a `Future`\*.

When a process is started, a **new context** will be assigned. Passed through the `init` closure and poll the future until it complete or abort. The future's output must be a `Result<(), ()>`.

```rs
use bastion::prelude::*;

fn main() {
    Bastion::init();
    Bastion::start();

    Bastion::children(|children| children.with_exec(|_ctx: BastionContext| async { Ok(()) }))
        .expect("Couldn't create the children group.");

    Bastion::stop();
}
```

\*`Actor Model`: Appendix - Definition
\*`Future`: Appendix - Definition
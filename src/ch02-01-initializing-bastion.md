# Initializing Bastion

## Initialize

Bastion require to be initialized. You can init it with the default configuration or with a custom one. A custom configuration will allow you to override the `Backtraces`\* system

Enabled by default, a custom configuration will allow you to override it and enable or disable the `Backtraces`.

- Default configuration

```rs
use bastion::prelude::*;

fn main() {
    Bastion::init();

    // Some stuff have to be written later... but not yet!
}
```

- Custom configuration, we are hidding the backtraces here, you can still use `show_backtraces();` to enable it.

```rs
use bastion::prelude::*;

fn main() {
    let config = Config::new().hide_backtraces();
    Bastion::init_with(config);

    // Some stuff have to be written later... but not yet!
}
```

## Start

After you initialize bastion you will have to start it by using `Bastion::start();`.

```rs
use bastion::prelude::*;

fn main() {
    Bastion::init();
    Bastion::start();

    // Some stuff have to be written later... but not yet!
}
```

## Block

As with any application, there will be a point where you want it to stop. But this time might not occur in your `main()` function. This can happen anywhere in your code.

Bastion provide `Bastion::block_until_stopped()`. It will block the current thread until the system is stopped (either by calling `Bastion::stop()` or `Bastion::kill()`).

```rs
use bastion::prelude::*;

fn main() {
    Bastion::init();
    Bastion::start();

    // Some stuff have to be written later... but not yet!

    Bastion::block_until_stopped();
}
```

## Stop

At this point you might want to stop the bastion. You can use `Bastion::stop()`.

It will send a message to the whole system to tell it to stop properly every running children groups and supervisors.

```rs
use bastion::prelude::*;

fn main() {
    Bastion::init();
    Bastion::start();

    // Some stuff have to be written later... but not yet!

    Bastion::stop();
}
```

As we said in the `Block` section of this chapter you may want to stop the application in an other part of your code. Here we will stop our bastion after the execution of `stop_me_now()`.

```rs
use bastion::prelude::*;

async fn stop_me_now(_ctx: BastionContext) -> Result<(), ()> {
    Bastion::stop();
    Ok(())
}

fn main() {
    Bastion::init();
    Bastion::start();

    Bastion::children(|children| children.with_exec(stop_me_now))
        .expect("Couldn't create the children group.");
}
```

As you may have noticed, we don't use `Bastion::block_until_stopped();` because the execution of `stop_me_now()` is not conditionned. We will talk about it later.

## Kill

In some case, in unrecoverable cases, you may want to just kill the system and drop all processes in execution. `Bastion::kill();` is made for it.

```rs
use bastion::prelude::*;

fn main() {
    Bastion::init();
    Bastion::start();

    // Some stuff have to be written later... but not yet!

    Bastion::kill();
}
```

\*`Backtraces`: Appendix - Definition

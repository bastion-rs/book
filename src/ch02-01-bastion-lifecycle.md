# Bastion lifecycle

## Initialize

Bastion require to be initialized. You can init it with the default configuration or with a custom one. A custom configuration will allow you to override the `Backtraces`* system

Enabled by default, a custom configuration will allow you to override it and enable or disable the `Backtraces`.

- Default configuration

```rs
use bastion::prelude::*;

fn main() {
    Bastion::init();
    // Some stuff have to be write after... but not yet!
}
```

- Custom configuration, we are hidding the backtraces here, you can still use `show_backtraces();` to enable it.

```rs
use bastion::prelude::*;

fn main() {
    let config = Config::new().hide_backtraces();
    Bastion::init_with(config);
    // Some stuff have to be write after... but not yet!
}
```

*`Backtraces`: Appendix - Definition

## Start

## Block

## Stop

## Kill
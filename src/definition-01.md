# Definition

Quick reminder to some notion that you will encounter.

## Backtraces

> To find the cause of something by examining past events.

In bastion we can override the rust backtrace to disable it.

- Backtrace enabled

```rs
use bastion::prelude::*;

fn main() {
    Bastion::init();

    panic!("Normal panic");
}
```

```
   Compiling bastion v0.4.5-alpha.0 (/home/bastion/src/bastion)
    Finished dev [unoptimized + debuginfo] target(s) in 0.79s
     Running `target/debug/examples/show_backtraces`
thread 'main' panicked at 'Normal panic', src/bastion/examples/show_backtraces.rs:6:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

- Backtrace disabled

```rs
use bastion::prelude::*;

fn main() {
    let config = Config::new().hide_backtraces();
    Bastion::init_with(config);
    
    panic!("Normal panic");
}
```

```
   Compiling bastion v0.4.5-alpha.0 (/home/bastion/src/bastion)
    Finished dev [unoptimized + debuginfo] target(s) in 0.79s
     Running `target/debug/examples/hide_backtraces`
```
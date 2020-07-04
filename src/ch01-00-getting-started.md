# Getting started

Let's start our journey with Bastion. During it we will see the main features, we have a lot to discover. For that we will follow a theme, we will build a small [CRUD application][CRUD].

In this first chapter, we will talk about:
- Installing Bastion on your project
- The Classic and timeless: hello, world!

## Installation

Bastion is a library which need to be added as a dependency to your project. For that, you just have to add it as follow to your `Cargo.toml`:
```rs
bastion = "0.3.5-alpha"
```
You can also use [cargo-edit][] and run `cargo add bastion` in a shell.
## Hello, world!
The most classic of all examples and especially the essential hello, world! Only for you, in Bastion.
```rs
fn main() {
    // We need bastion to run our program
    Bastion::init();
    // We are starting the Bastion program now
    Bastion::start();
    // We are creating the group of children which will received the message
    let children = Bastion::children(|children| {
        // We create the function to exec
        children.with_exec(|ctx: BastionContext| {
            async move {
                msg! {
                  // We are waiting a msg
                  ctx.recv().await?,
                  ref msg: &'static str => {
                    // We are logging the msg broadcasted bellow
                    println!("{}", msg);
                  };
                  _: _ => ();
                }
                // We are stopping bastion here
                Bastion::stop();
                Ok(())
            }
        })
    })
    .expect("Couldn't create the children group.");
    // We are creating the message to broadcast to the children group
    children
        .broadcast("Hello, world!")
        .expect("Couldn't broadcast the message.");
    // We are waiting until the Bastion has stopped or got killed
    Bastion::block_until_stopped();
}
```

Don't forget, if you have any question, you can find us on [Discord][]!

[CRUD]: https://en.wikipedia.org/wiki/Create,_read,_update_and_delete
[cargo-edit]: https://github.com/killercup/cargo-edit
[Discord]: https://discord.gg/DqRqtRT

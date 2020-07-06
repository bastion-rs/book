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
use bastion::prelude::*;

fn main() {
    // We need bastion to run our program
    Bastion::init();
    Bastion::start();
    // We are creating a group of children
    let workers = Bastion::children(|children| {
        // We are creating the function to exec
        children.with_exec(|ctx: BastionContext| {
            async move {
                // We are defining a behavior when a msg is received
                msg! {
                    // We are waiting a msg
                    ctx.recv().await?,
                    // We are catching a msg
                    msg: &'static str =!> {
                        // Printing the incoming msg
                        println!("{}", msg);
                        // Sending the asnwer
                        answer!(ctx, msg).expect("couldn't reply :(");
                    };
                    _: _ => ();
                }
                Ok(())
            }
        })
    })
    .expect("Couldn't create the children group.");
    // We are creating the asker
    let asker = async {
        // We are getting the first (and only) worker
        let answer = workers.elems()[0]
            .ask_anonymously("hello, world!")
            .expect("Couldn't send the message.");
        // We are waiting for the asnwer
        answer.await.expect("couldn't receive answer");
    };
    // We are running the asker in the current blocked thread
    run!(asker);
    // We are stopping bastion here
    Bastion::stop();
}
```

Don't forget, if you have any question, you can find us on [Discord][]!

[CRUD]: https://en.wikipedia.org/wiki/Create,_read,_update_and_delete
[cargo-edit]: https://github.com/killercup/cargo-edit
[Discord]: https://discord.gg/DqRqtRT

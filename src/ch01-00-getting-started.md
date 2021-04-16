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

async fn once_hello_world(ctx: BastionContext) -> Result<(), ()> {
    MessageHandler::new(ctx.recv().await?)
        .on_question(|question: &str, sender| {
            if question == "hi!" {
                sender
                    .reply("hello, world!")
                    .expect("Failed to reply to sender");
            } else {
                panic!("Expected string `hi!`, found `{}`", question);
            }
        })
        .on_fallback(|v, addr| panic!("Wrong message from {:?}: got {:?}", addr, v));
    Ok(())
}

fn main() {
    Bastion::init();
    Bastion::start();

    Bastion::children(|children| {
        children
            .with_distributor(Distributor::named("say_hi"))
            .with_exec(once_hello_world)
    })
    .expect("Couldn't create the children group.");

    let say_hi = Distributor::named("say_hi");

    run!(async {
        let answer: Result<&str, SendError> =
            say_hi.request("hi!").await.expect("Couldn't send request");

        println!("{}", answer.expect("Couldn't receive answer"))
    });

    Bastion::stop();
}
```

Don't forget, if you have any question, you can find us on [Discord][]!

[CRUD]: https://en.wikipedia.org/wiki/Create,_read,_update_and_delete
[cargo-edit]: https://github.com/killercup/cargo-edit
[Discord]: https://discord.gg/DqRqtRT

# Running A System By An Event

We can turn on a system by an event.
More precisely, we can run a system whenever a certain type of events is triggered.

This can be done by the function [on_event](https://docs.rs/bevy/latest/bevy/ecs/schedule/common_conditions/fn.on_event.html).
For example, if we consider the event [KeyboardInput](https://docs.rs/bevy/latest/bevy/input/keyboard/struct.KeyboardInput.html):

```rust
some_system.run_if(on_event::<KeyboardInput>())
```

In the following code, when we press any keys, the app runs a system that prints a string `A key event occurred.`.

```rust
use bevy::{
    app::{App, Update},
    ecs::schedule::{common_conditions::on_event, IntoSystemConfigs},
    input::keyboard::KeyboardInput,
    DefaultPlugins,
};

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_systems(Update, print.run_if(on_event::<KeyboardInput>()))
        .run();
}

fn print() {
    println!("A key event occurred.");
}
```

When a key is pressed, the string `A key event occurred.` is appended to the output console.

In addition to [KeyboardInput](https://docs.rs/bevy/latest/bevy/input/keyboard/struct.KeyboardInput.html), we can use any other events including custom events.

:arrow_right:  Next: [Using The State Machine](./using_the_state_machine.md)

:blue_book: Back: [Table of contents](./../README.md)

# They Are Functions In Bevy

To execute a function in [Bevy](https://bevyengine.org/), we need to add a [System](https://docs.rs/bevy/latest/bevy/ecs/system/trait.System.html) to the [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html) through the [add_systems](https://docs.rs/bevy/latest/bevy/app/struct.App.html#method.add_systems) method.
One type of the [System](https://docs.rs/bevy/latest/bevy/ecs/system/trait.System.html) is a function without parameters.
We pass the name of the function (`hello` in the following code) to the [add_systems](https://docs.rs/bevy/latest/bevy/app/struct.App.html#method.add_systems) method.

```rust
use bevy::app::{App, Startup};

fn main() {
    App::new().add_systems(Startup, hello).run();
}

fn hello() {
    println!("Hello world!");
}
```

Output:

```text
Hello world!
```

In addition to the name of the function, we also specify when to execute the function.
In the code above, we specify [Startup](https://docs.rs/bevy/latest/bevy/app/struct.Startup.html) as the moment to execute the `hello` function.

:arrow_right:  Next: [The Default Scheduler For Systems](./the_default_scheduler_for_systems.md)

:blue_book: Back: [Table of contents](./../README.md)

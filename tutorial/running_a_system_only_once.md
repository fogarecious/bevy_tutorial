# Running A System Only Once

We can use [run_if](https://docs.rs/bevy/latest/bevy/ecs/schedule/trait.IntoSystemConfigs.html#method.run_if) to restrict a system to run only once even if the system is in the [Update](https://docs.rs/bevy/latest/bevy/app/struct.Update.html) schedule.
This is done by the function [run_once()](https://docs.rs/bevy/latest/bevy/ecs/schedule/common_conditions/fn.run_once.html).

```rust
use bevy::{
    app::{App, Update},
    ecs::schedule::{common_conditions::run_once, IntoSystemConfigs},
    DefaultPlugins,
};

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_systems(Update, print.run_if(run_once()))
        .run();
}

fn print() {
    println!("A system that runs once.");
}
```

Output:

```text
A system that runs once.
```

:arrow_right:  Next: [Camera 3D](./camera_3d.md)

:blue_book: Back: [Table of contents](./../README.md)

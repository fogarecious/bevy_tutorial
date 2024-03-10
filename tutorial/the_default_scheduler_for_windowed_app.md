# The Default Scheduler For Windowed App

Previously, we mentioned that we can add a system to our app by the [add_systems](https://docs.rs/bevy/latest/bevy/app/struct.App.html#method.add_systems) method of [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html).
The [add_systems](https://docs.rs/bevy/latest/bevy/app/struct.App.html#method.add_systems) is associated with a schedule label, such as [Startup](https://docs.rs/bevy/latest/bevy/app/struct.Startup.html), [Update](https://docs.rs/bevy/latest/bevy/app/struct.Update.html), etc.
The schedule labels are considered in a predetermined sequence.
The [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html) exits after each schedule label has been considered.

The [DefaultPlugins](https://docs.rs/bevy/latest/bevy/struct.DefaultPlugins.html) not only adds a window, but also changes the order of schedule labels.
The schedule labels are now arranged as a loop.
So the app is able to run until the window is closed.

The new order of schedule labels is demonstrated by the following example.

```rust
use bevy::{
    app::{
        App, First, FixedUpdate, Last, PostStartup, PostUpdate, PreStartup, PreUpdate,
        RunFixedUpdateLoop, Startup, StateTransition, Update,
    },
    DefaultPlugins,
};

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_systems(First, first)
        .add_systems(FixedUpdate, fixed_update)
        .add_systems(Last, last)
        .add_systems(PostStartup, post_startup)
        .add_systems(PostUpdate, post_update)
        .add_systems(PreStartup, pre_startup)
        .add_systems(PreUpdate, pre_update)
        .add_systems(RunFixedUpdateLoop, run_fixed_update_loop)
        .add_systems(Startup, startup)
        .add_systems(StateTransition, state_transition)
        .add_systems(Update, update)
        .run();
}

fn first() {
    println!("First");
}

fn fixed_update() {
    println!("FixedUpdate");
}

fn last() {
    println!("Last");
}

fn post_startup() {
    println!("PostStartup");
}

fn post_update() {
    println!("PostUpdate");
}

fn pre_startup() {
    println!("PreStartup");
}

fn pre_update() {
    println!("PreUpdate");
}

fn run_fixed_update_loop() {
    println!("RunFixedUpdateLoop");
}

fn startup() {
    println!("Startup");
}

fn state_transition() {
    println!("StateTransition");
}

fn update() {
    println!("Update");
}
```

Output:

```text
PreStartup
Startup
PostStartup
First
PreUpdate
StateTransition
RunFixedUpdateLoop
Update
PostUpdate
Last
First
PreUpdate
StateTransition
RunFixedUpdateLoop
FixedUpdate
FixedUpdate
FixedUpdate
Update
PostUpdate
Last
First
PreUpdate
StateTransition
RunFixedUpdateLoop
FixedUpdate
FixedUpdate
FixedUpdate
FixedUpdate
FixedUpdate
FixedUpdate
FixedUpdate
FixedUpdate
FixedUpdate
FixedUpdate
FixedUpdate
FixedUpdate
FixedUpdate
FixedUpdate
FixedUpdate
FixedUpdate
Update
PostUpdate
Last
...
```

The rest of schedule labels repeats from [First](https://docs.rs/bevy/latest/bevy/app/struct.First.html) to [Last](https://docs.rs/bevy/latest/bevy/app/struct.Last.html), in which [FixedUpdate](https://docs.rs/bevy/latest/bevy/app/struct.FixedUpdate.html) may repeat several times.

:arrow_right:  Next: [Initializing A Different Window](./initializing_a_different_window.md)

:blue_book: Back: [Table of contents](./../README.md)

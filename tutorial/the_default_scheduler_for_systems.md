# The Default Scheduler For Systems

Previously, we use [add_systems](https://docs.rs/bevy/latest/bevy/app/struct.App.html#method.add_systems) to execute a function.

```rust
add_systems(Startup, hello)
```

The [Startup](https://docs.rs/bevy/latest/bevy/app/struct.Startup.html) struct indicates the moment when function `hello` is executed.
There are other structs that help us executing functions at different moments, as shown in the following code.
These structs can be found in the description of the [Main](https://docs.rs/bevy/latest/bevy/app/struct.Main.html) schedule.

```rust
use bevy::app::{
    App, First, Last, PostStartup, PostUpdate, PreStartup, PreUpdate, RunFixedUpdateLoop, Startup,
    StateTransition, Update,
};

fn main() {
    App::new()
        .add_systems(First, first)
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
```

:arrow_right:  Next: [Executing Multiple Systems Simultaneously](./executing_multiple_systems_simultaneously.md)

:blue_book: Back: [Table of contents](./../README.md)

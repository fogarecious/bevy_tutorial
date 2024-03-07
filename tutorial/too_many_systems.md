# Too Many Systems

[System](https://docs.rs/bevy/latest/bevy/ecs/system/trait.System.html)s are important in the [Bevy](https://bevyengine.org/) engine.
When our project is getting larger and larger, we may need to add many systems.
For example, in our tutorial for [The Default Scheduler For Systems](./the_default_scheduler_for_systems.md), we added 10 systems.

```rust
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
```

This makes the `main` function long and unclear.

To solve this problem, we can move these codes that add systems into a [Plugin](https://docs.rs/bevy/latest/bevy/app/trait.Plugin.html).

```rust
struct MyPlugin;

impl Plugin for MyPlugin {
    fn build(&self, app: &mut App) {
        app.add_systems(First, first)
            .add_systems(Last, last)
            .add_systems(PostStartup, post_startup)
            .add_systems(PostUpdate, post_update)
            .add_systems(PreStartup, pre_startup)
            .add_systems(PreUpdate, pre_update)
            .add_systems(RunFixedUpdateLoop, run_fixed_update_loop)
            .add_systems(Startup, startup)
            .add_systems(StateTransition, state_transition)
            .add_systems(Update, update);
    }
}
```

Our main function is simplified.

```rust
fn main() {
    App::new().add_plugins(MyPlugin).run();
}
```

The full code is as follows:

```rust
use bevy::app::{
    App, First, Last, Plugin, PostStartup, PostUpdate, PreStartup, PreUpdate, RunFixedUpdateLoop,
    Startup, StateTransition, Update,
};

fn main() {
    App::new().add_plugins(MyPlugin).run();
}

struct MyPlugin;

impl Plugin for MyPlugin {
    fn build(&self, app: &mut App) {
        app.add_systems(First, first)
            .add_systems(Last, last)
            .add_systems(PostStartup, post_startup)
            .add_systems(PostUpdate, post_update)
            .add_systems(PreStartup, pre_startup)
            .add_systems(PreUpdate, pre_update)
            .add_systems(RunFixedUpdateLoop, run_fixed_update_loop)
            .add_systems(Startup, startup)
            .add_systems(StateTransition, state_transition)
            .add_systems(Update, update);
    }
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

We get the same output.

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

:arrow_right:  Next: [Faster Compile Time](./faster_compile_time.md)

:blue_book: Back: [Table of contents](./../README.md)

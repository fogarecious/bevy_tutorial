# Using The State Machine

[Bevy](https://bevyengine.org/) has a build-in state management that helps us to execute systems only in some states.

In the following example, we create the state machine containing only one state.
When the system is in this state (which is always true in the example), it prints `In MainState`.

First, we create a enum `MyStates` that derives [States](https://docs.rs/bevy/latest/bevy/ecs/schedule/derive.States.html).

```rust
#[derive(States, Default, Debug, Clone, Eq, PartialEq, Hash)]
enum MyStates {
    #[default]
    MainState,
}
```

The enum `MyStates` also derives [Default](https://doc.rust-lang.org/core/default/derive.Default.html), [Debug](https://doc.rust-lang.org/std/fmt/derive.Debug.html), [Clone](https://doc.rust-lang.org/std/clone/derive.Clone.html), [Eq](https://doc.rust-lang.org/std/cmp/derive.Eq.html), [PartialEq](https://doc.rust-lang.org/core/cmp/derive.PartialEq.html) and [Hash](https://doc.rust-lang.org/core/hash/derive.Hash.html), as required by the underlying system.
Additionally, we mark `MainState` as the default state by `#[default]`.

Then we add `MyStates` to our [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html).

```rust
App::new().add_state::<MyStates>().add_systems(Update, print.run_if(in_state(MyStates::MainState)))
```

We use the method [add_state](https://docs.rs/bevy/0.12.1/bevy/app/struct.App.html#method.add_state) of [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html) to add the states.
It will be initialized to the default state.
For the system `print`, we turn it on when the system is in the state `MyStates::MainState`.
We use the function [in_state](https://docs.rs/bevy/latest/bevy/ecs/schedule/common_conditions/fn.in_state.html) to check if it is the specified state.

The full code is as follows:

```rust
use bevy::{
    app::{App, Update},
    ecs::schedule::{common_conditions::in_state, IntoSystemConfigs, States},
    DefaultPlugins,
};

#[derive(States, Default, Debug, Clone, Eq, PartialEq, Hash)]
enum MyStates {
    #[default]
    MainState,
}

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_state::<MyStates>()
        .add_systems(Update, print.run_if(in_state(MyStates::MainState)))
        .run();
}

fn print() {
    println!("In MainState");
}
```

Output:

```text
In MainState
In MainState
In MainState
... (repeat infinitely)
```

In the next tutorial, we will show how to manipulate states.

<!-- :arrow_right:  Next:  -->

:blue_book: Back: [Table of contents](./../README.md)

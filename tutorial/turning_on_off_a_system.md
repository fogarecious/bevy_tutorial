# Turning On/Off A System

In addition to schedule labels, such as [Startup](https://docs.rs/bevy/latest/bevy/app/struct.Startup.html) and [Update](https://docs.rs/bevy/latest/bevy/app/struct.Update.html), we can have more control on [System](https://docs.rs/bevy/latest/bevy/ecs/system/trait.System.html)s.
There are functions/methods located at [bevy::ecs::schedule::common_conditions](https://docs.rs/bevy/latest/bevy/ecs/schedule/common_conditions/index.html) as well as [IntoSystemConfigs](https://docs.rs/bevy/latest/bevy/ecs/schedule/trait.IntoSystemConfigs.html#) that help us to control systems.

In the following example, we add two [System](https://docs.rs/bevy/latest/bevy/ecs/system/trait.System.html)s to the moment [Startup](https://docs.rs/bevy/latest/bevy/app/struct.Startup.html).
However, only one of the systems will actually run.
The other system will not run.

We use the function [run_if](https://docs.rs/bevy/latest/bevy/ecs/schedule/trait.IntoSystemConfigs.html#method.run_if) to set a condition specifying when the system can be run.

```rust
add_systems(Startup, print_a.run_if(always_true))
```

The function [run_if](https://docs.rs/bevy/latest/bevy/ecs/schedule/trait.IntoSystemConfigs.html#method.run_if) takes a system, `always_true` here, which returns a `bool` indicating whether the system should be run.
The system `always_true` is a simple system created by us.

```rust
fn always_true() -> bool {
    return true;
}
```

When a system is acted as a condition, it can be preceded with the function [not](https://docs.rs/bevy/latest/bevy/ecs/schedule/common_conditions/fn.not.html), to inverse its output.

```rust
add_systems(Startup, print_b.run_if(not(always_true)))
```

The full code is as follows:

```rust
use bevy::{
    app::{App, Startup},
    ecs::schedule::{common_conditions::not, IntoSystemConfigs},
};

fn main() {
    App::new()
        .add_systems(
            Startup,
            (
                print_a.run_if(always_true),
                print_b.run_if(not(always_true)),
            ),
        )
        .run();
}

fn print_a() {
    println!("A");
}

fn print_b() {
    println!("B");
}

fn always_true() -> bool {
    return true;
}
```

Output:

```text
A
```

In the output, only `A` is printed, and `B` is not printed.
This means `print_a` is executed, but `print_b` is not.

:arrow_right:  Next: [Running A System By An Event](./running_a_system_by_an_event.md)

:blue_book: Back: [Table of contents](./../README.md)

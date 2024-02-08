# Executing Multiple Systems Set By Set

As the number of systems increases in a project, it becomes harder and harder to maintain their execution orders, especially when we have to specify the orders one system at a time.
[Bevy](https://bevyengine.org/) provides the [SystemSet](https://docs.rs/bevy/latest/bevy/ecs/schedule/trait.SystemSet.html) trait that helps us grouping a set of systems.
Then we can specify the orders among these system sets.

To do so, we need to have an enum that derives the [SystemSet](https://docs.rs/bevy/latest/bevy/ecs/schedule/derive.SystemSet.html) macro.
The enum acts as labels of the system sets.
Then we use the [configure_sets](https://docs.rs/bevy/latest/bevy/app/struct.App.html#method.configure_sets) method of [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html) to specify the orders of the system sets.
Finally, we use the [in_set](https://docs.rs/bevy/latest/bevy/ecs/schedule/trait.IntoSystemSetConfigs.html#method.in_set) method of [IntoSystemSetConfigs](https://docs.rs/bevy/latest/bevy/ecs/schedule/trait.IntoSystemSetConfigs.html) to specify relations between systems and the system sets.

```rust
use bevy::{
    app::{App, Startup},
    ecs::schedule::{IntoSystemConfigs, IntoSystemSetConfigs, SystemSet},
};

#[derive(SystemSet, Debug, Clone, Hash, Eq, PartialEq)]
enum MySet {
    A,
    B,
}

fn main() {
    App::new()
        .configure_sets(Startup, (MySet::A, MySet::B).chain())
        .add_systems(
            Startup,
            (
                hello_a1.in_set(MySet::A),
                hello_a2.in_set(MySet::A),
                hello_b1.in_set(MySet::B),
                hello_b2.in_set(MySet::B),
            ),
        )
        .run();
}

fn hello_a1() {
    println!("Hello A1!");
}

fn hello_a2() {
    println!("Hello A2!");
}

fn hello_b1() {
    println!("Hello B1!");
}

fn hello_b2() {
    println!("Hello B2!");
}
```

In the output, `Hello A1!` and `Hello A2!` must be printed before `Hello B1!` and `Hello B2!`.
Yet, the order between `Hello A1!` and `Hello A2!` (as well as `Hello B1!` and `Hello B2!`) is uncertain.

To compile the program successfully, any enum deriving [SystemSet](https://docs.rs/bevy/latest/bevy/ecs/schedule/derive.SystemSet.html) must also derive [Debug](https://doc.rust-lang.org/std/fmt/derive.Debug.html), [Clone](https://doc.rust-lang.org/std/clone/derive.Clone.html), [Hash](https://doc.rust-lang.org/std/hash/derive.Hash.html), [Eq](https://doc.rust-lang.org/std/cmp/derive.Eq.html), and [PartialEq](https://doc.rust-lang.org/std/cmp/derive.PartialEq.html).

By using [SystemSet](https://docs.rs/bevy/latest/bevy/ecs/schedule/trait.SystemSet.html), we can easily swap the order of the system sets.

```rust
configure_sets(Startup, (MySet::B, MySet::A).chain())
```

:arrow_right:  Next: [Resources - They Are Singleton Structs](./they_are_singleton_structs.md)

:blue_book: Back: [Table of contents](./../README.md)

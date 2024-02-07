# Searching For Entities By Components

Just like relational databases, the data in the [Bevy](https://bevyengine.org/) table can also be searched.

We use the [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html) struct in system parameters to search the data in the table.
When being a type of a variable, [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html) needs a *type parameter* to specify which component(s) it is going to search for.
For example, `Query<&Hp>` helps us searching for all entities that have `Hp` as one of its [components](https://docs.rs/bevy/latest/bevy/ecs/component/trait.Component.html).
Note that the type parameter should be declared as a reference, with `&` ahead.

```rust
fn output_names(names: Query<&Name>) {
    println!("All names:");

    for name in &names {
        println!("{}", name.0);
    }

    println!();
}

fn output_hps(hps: Query<&Hp>) {
    println!("All HPs:");

    for hp in &hps {
        println!("{}", hp.0);
    }

    println!();
}
```

A variable of type [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html) can be treated as an iterator and thus can be put in a for loop.

To search for entities with multiple components, put the components in a pair of parentheses and pass the parentheses as a type parameter to [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html).

```rust
fn output_names_and_hps(players: Query<(&Name, &Hp)>) {
    println!("All players:");

    for (name, hp) in &players {
        println!("Name: {}, HP: {}", name.0, hp.0);
    }

    println!();
}
```

Similar to systems, the number of [components](https://docs.rs/bevy/latest/bevy/ecs/component/trait.Component.html) in the parentheses is limited to 15, yet recursion is possible.

Now we can call the three systems above as follows:

```rust
use bevy::{
    app::{App, Startup, Update},
    ecs::{
        component::Component,
        schedule::IntoSystemConfigs,
        system::{Commands, Query},
    },
};

fn main() {
    App::new()
        .add_systems(Startup, add_players)
        .add_systems(
            Update,
            (output_names, output_hps, output_names_and_hps).chain(),
        )
        .run();
}

#[derive(Component)]
struct Name(String);

#[derive(Component)]
struct Hp(u32);

fn add_players(mut commands: Commands) {
    commands.spawn((Name("Soldier 1".into()), Hp(100)));
    commands.spawn((Name("Soldier 2".into()), Hp(250)));
    commands.spawn((Name("Soldier 3".into()), Hp(150)));
}

// the code for output_names, output_hps, and output_names_and_hps
```

We use [chain](https://docs.rs/bevy/latest/bevy/ecs/schedule/trait.IntoSystemConfigs.html#method.chain) to make an order of the three systems.
Otherwise, the output will be interleaved.

```text
All names:
Soldier 1
Soldier 2
Soldier 3

All HPs:
100
250
150

All players:
Name: Soldier 1, HP: 100
Name: Soldier 2, HP: 250
Name: Soldier 3, HP: 150

```

:arrow_right:  Next: [Entities Can Have Different Components](./entities_can_have_different_components.md)

:blue_book: Back: [Table of contents](./../README.md)

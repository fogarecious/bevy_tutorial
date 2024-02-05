# They Are Like Tables

The internal data structure of [Bevy](https://bevyengine.org/) is like a table.
One of the power of [Bevy](https://bevyengine.org/) is to let us manipulate the table freely.

For example, let's say we have three players in our app.
The table looks like this:

| Name | HP |
| ---- | -- |
| Soldier 1 | 100 |
| Soldier 2 | 250 |
| Soldier 3 | 150 |

Each row in the table is called an *entity*, and each column is called a *component*.

A component is basically a struct in [Rust](https://www.rust-lang.org/) that derives the [Component](https://docs.rs/bevy/latest/bevy/ecs/component/derive.Component.html) macro.
In our table, we have two components: `Name` and `HP`.

```rust
use bevy::ecs::component::Component;

// ...

#[derive(Component)]
struct Name(String);

#[derive(Component)]
struct Hp(u32);
```

We can use the [spawn](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html#method.spawn) method of [Commands](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html) to add an entity to the [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html).
([Commands](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html) were introduced in the previous tutorial for [Removing Resources](./removing_resources.md).)

```rust
use bevy::{
    app::{App, Startup},
    ecs::{component::Component, system::Commands},
};

fn main() {
    App::new().add_systems(Startup, add_players).run();
}

// ...

fn add_players(mut commands: Commands) {
    commands.spawn((Name("Soldier 1".into()), Hp(100)));
    commands.spawn((Name("Soldier 2".into()), Hp(250)));
    commands.spawn((Name("Soldier 3".into()), Hp(150)));
}
```

We put some specified components in a pair of parentheses and pass the parentheses to the [spawn](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html#method.spawn) method.
Each [spawn](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html#method.spawn) creates an entity and add it to the [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html).

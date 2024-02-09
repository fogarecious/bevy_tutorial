# Bundles Help Us Grouping Components Together

Since entities can have different components, sometimes we might forget what components we are going to use.
[Bundle](https://docs.rs/bevy/latest/bevy/ecs/bundle/derive.Bundle.html) helps us grouping a set of components so that we can use Rust's type checking to [spawn](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html#method.spawn) the same type of entities.

Previously, we have two types of entities in mind: soldier and wizard.
Now, we explicitly define these types by [Bundle](https://docs.rs/bevy/latest/bevy/ecs/bundle/derive.Bundle.html).

```rust
#[derive(Component)]
struct Name(String);

#[derive(Component)]
struct Hp(u32);

#[derive(Component)]
struct Mp(u32);

#[derive(Bundle)]
struct SoldierBundle {
    name: Name,
    hp: Hp,
}

#[derive(Bundle)]
struct WizardBundle {
    name: Name,
    hp: Hp,
    mp: Mp,
}
```

Then we can [spawn](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html#method.spawn) these bundles.

```rust
use bevy::{
    app::{App, Startup, Update},
    ecs::{
        bundle::Bundle,
        component::Component,
        system::{Commands, Query},
    },
};

fn main() {
    App::new()
        .add_systems(Startup, add_players)
        .add_systems(Update, output_players)
        .run();
}

// the code for the structs of components and bundles

fn add_players(mut commands: Commands) {
    commands.spawn(SoldierBundle {
        name: Name("Soldier 1".into()),
        hp: Hp(100),
    });
    commands.spawn(SoldierBundle {
        name: Name("Soldier 2".into()),
        hp: Hp(250),
    });
    commands.spawn(SoldierBundle {
        name: Name("Soldier 3".into()),
        hp: Hp(150),
    });

    commands.spawn(WizardBundle {
        name: Name("Wizard 1".into()),
        hp: Hp(50),
        mp: Mp(30),
    });
    commands.spawn(WizardBundle {
        name: Name("Wizard 2".into()),
        hp: Hp(30),
        mp: Mp(60),
    });
}

fn output_players(players: Query<(&Name, &Hp)>) {
    println!("All players:");

    for (name, hp) in &players {
        println!("Name: {}, HP: {}", name.0, hp.0);
    }

    println!();
}
```

Output:

```text
All players:
Name: Soldier 1, HP: 100
Name: Soldier 2, HP: 250
Name: Soldier 3, HP: 150
Name: Wizard 1, HP: 50
Name: Wizard 2, HP: 30
```

Spawning a bundle is equivalent to spawn a tuple of its inner components.

:arrow_right:  Next: [Searching For Entities By Optional Components](./searching_for_entities_by_optional_components.md)

:blue_book: Back: [Table of contents](./../README.md)

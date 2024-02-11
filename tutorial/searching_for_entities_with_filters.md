# Searching For Entities With Filters

Previously, we mentioned that we can use unique components to filter the [Bevy](https://bevyengine.org/) table.
But how can we do if we really want to select a particular type of entities?
For example, to select soldiers only, or to select wizards only.

To do this, we can use the [With](https://docs.rs/bevy/latest/bevy/ecs/query/struct.With.html) and the [Without](https://docs.rs/bevy/latest/bevy/ecs/query/struct.Without.html) struct in the type parameters of [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html).
For instance, by selecting all entities that have no MP component, we can select the soldiers only.

```rust
fn output_soldiers(soldiers: Query<(&Name, &Hp), Without<Mp>>) {
    println!("All soldiers:");

    for (name, hp) in &soldiers {
        println!("Name: {}, HP: {}", name.0, hp.0);
    }

    println!();
}
```

We add `Without<Mp>` as the second parameter of [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html).
This query only selects the entities that have no `Mp` component.
Note that the `Mp` in `Without<Mp>` has no `&` attached ahead.

In addition, when we want to select all wizards, but we do not need to read the MP component (for performance issues in some cases), we can do the following:

```rust
fn output_wizards(wizards: Query<(&Name, &Hp), With<Mp>>) {
    println!("All wizards:");

    for (name, hp) in &wizards {
        println!("Name: {}, HP: {}", name.0, hp.0);
    }

    println!();
}
```

This query selects all wizards only, since the wizards are the only entities having the MP component.
Yet, the [Bevy](https://bevyengine.org/) engine does not prepare and read the data of the wizards' MP component.
This makes the system more efficient.

If we need the MP component of the wizards, we can use `Query<(&Name, &Hp, &Mp)` as we discussed before.

The full code is as follows:

```rust
use bevy::{
    app::{App, Startup, Update},
    ecs::{
        bundle::Bundle,
        component::Component,
        query::{With, Without},
        schedule::IntoSystemConfigs,
        system::{Commands, Query},
    },
};

fn main() {
    App::new()
        .add_systems(Startup, add_players)
        .add_systems(Update, (output_soldiers, output_wizards).chain())
        .run();
}

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

// the code of output_soldiers and output_wizards
```

Output:

```text
All soldiers:
Name: Soldier 1, HP: 100
Name: Soldier 2, HP: 250
Name: Soldier 3, HP: 150

All wizards:
Name: Wizard 1, HP: 50
Name: Wizard 2, HP: 30

```

:arrow_right:  Next: [Searching For The Only Entity](./searching_for_the_only_entity.md)

:blue_book: Back: [Table of contents](./../README.md)

# Searching For Entities By Optional Components

Previously, we have two types of entities: the soldiers and the wizards.
The types differ in the MP component.

Now, we want to print all these entities.
If an entity is of type wizards (which have the MP component), we print the MP component out together with other components.
Otherwise, we only print the other components.
Let's see how to do this in the following example.

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

fn output_players(players: Query<(&Name, &Hp, Option<&Mp>)>) {
    println!("All players:");

    for (name, hp, mp) in &players {
        print!("Name: {}, HP: {}", name.0, hp.0);
        match mp {
            Some(v) => println!(", MP: {}", v.0),
            None => println!(),
        }
    }

    println!();
}
```

In the parentheses that we pass to [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html), we have one more parameter: `Option<&Mp>`.
The parameter tells [Bevy](https://bevyengine.org/) the `Mp` [Component](https://docs.rs/bevy/latest/bevy/ecs/component/trait.Component.html) is optional.
Then we treat the MP variable as of type `Option<&Mp>`.

Output:

```text
All players:
Name: Soldier 1, HP: 100
Name: Soldier 2, HP: 250
Name: Soldier 3, HP: 150
Name: Wizard 1, HP: 50, MP: 30
Name: Wizard 2, HP: 30, MP: 60

```

:arrow_right:  Next: [Searching For Entities With Filters](./searching_for_entities_with_filters.md)

:blue_book: Back: [Table of contents](./../README.md)

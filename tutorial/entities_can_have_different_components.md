# Entities Can Have Different Components

Now, let's add some more players.

| Name | HP | MP |
| ---- | -- | -- |
| Soldier 1 | 100 | |
| Soldier 2 | 250 | |
| Soldier 3 | 150 | |
| Wizard 1 | 50 | 30 |
| Wizard 2 | 30 | 60 |

We add two wizards who have magic power (with MP).

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
        .add_systems(Update, (output_names_and_hps, output_names_and_mps).chain())
        .run();
}

#[derive(Component)]
struct Name(String);

#[derive(Component)]
struct Hp(u32);

#[derive(Component)]
struct Mp(u32);

fn add_players(mut commands: Commands) {
    commands.spawn((Name("Soldier 1".into()), Hp(100)));
    commands.spawn((Name("Soldier 2".into()), Hp(250)));
    commands.spawn((Name("Soldier 3".into()), Hp(150)));

    commands.spawn((Name("Wizard 1".into()), Hp(50), Mp(30)));
    commands.spawn((Name("Wizard 2".into()), Hp(30), Mp(60)));
}
```

We have a new [Component](https://docs.rs/bevy/latest/bevy/ecs/component/trait.Component.html), `Mp`.
This component helps us [spawn](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html#method.spawn) two wizard entities in the `add_players` system.

Note that the soldier entities have *no* `Mp` component.
We can, of course, add an `Mp` component with value 0 for them.
But let us see what will happen if they have no `Mp` component.

First, we print all the players who have name and hp.

```rust
fn output_names_and_hps(players_with_name_and_hp: Query<(&Name, &Hp)>) {
    println!("All players with name and hp:");

    for (name, hp) in &players_with_name_and_hp {
        println!("Name: {}, HP: {}", name.0, hp.0);
    }

    println!();
}
```

Output:

```text
All players with name and hp:
Name: Soldier 1, HP: 100
Name: Soldier 2, HP: 250
Name: Soldier 3, HP: 150
Name: Wizard 1, HP: 50
Name: Wizard 2, HP: 30
```

Since all players have `Name` and `Hp`, they are all printed.

Next, we print all the players who have name and mp.

```rust
fn output_names_and_mps(players_with_name_and_mp: Query<(&Name, &Mp)>) {
    println!("All players with name and mp:");

    for (name, mp) in &players_with_name_and_mp {
        println!("Name: {}, MP: {}", name.0, mp.0);
    }

    println!();
}
```

Output:

```text
All players with name and mp:
Name: Wizard 1, MP: 30
Name: Wizard 2, MP: 60
```

Since the wizards are the only players having `Mp`, they are printed.
To get the same result, we can also use `Query<(&Name, &HP, &Mp)>` instead of `Query<(&Name, &Mp)>`.

Adding unique components helps us having basic filtering of the entities.

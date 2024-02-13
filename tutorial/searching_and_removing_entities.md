# Searching And Removing Entities

In addition to update entities, we can remove entities from the [Bevy](https://bevyengine.org/) table.

Each entity in [Bevy](https://bevyengine.org/) has an id, which is of type [Entity](https://docs.rs/bevy/latest/bevy/ecs/entity/struct.Entity.html).
To remove an entity, we have to first find the id of the entity.
Then, we use the id to find the entity through [Commands](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html) and remove the entity by the [despawn](https://docs.rs/bevy/latest/bevy/ecs/system/struct.EntityCommands.html#method.despawn) method of the entity.

```rust
#[derive(Component)]
struct Name(String);

#[derive(Component)]
struct Hp(u32);

#[derive(Component)]
struct Enemy;

fn remove_enemies(mut commands: Commands, enemies: Query<(Entity, &Name), With<Enemy>>) {
    for (id, name) in &enemies {
        println!("{} is dead.", name.0);
        commands.entity(id).despawn();
    }

    println!();
}
```

To find the id of an entity, we pass [Entity](https://docs.rs/bevy/latest/bevy/ecs/entity/struct.Entity.html) as a type parameter to [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html).
Note that the `Entity` has no `&` attached ahead.
Then, we use the [entity](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html#method.entity) method of [Commands](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html) to retrieve the entity.

The full code is as follows:

```rust
use bevy::{
    app::{App, PostUpdate, PreUpdate, Startup, Update},
    ecs::{
        component::Component,
        entity::Entity,
        query::With,
        system::{Commands, Query},
    },
};

fn main() {
    App::new()
        .add_systems(Startup, add_players)
        .add_systems(PreUpdate, output_players)
        .add_systems(Update, remove_enemies)
        .add_systems(PostUpdate, output_players)
        .run();
}

// the code for the components: Name, Hp, and Enemy

fn add_players(mut commands: Commands) {
    commands.spawn((Name("Friend 1".into()), Hp(100)));
    commands.spawn((Name("Friend 2".into()), Hp(250)));
    commands.spawn((Name("Friend 3".into()), Hp(150)));

    commands.spawn((Name("Enemy 1".into()), Hp(300), Enemy));
    commands.spawn((Name("Enemy 2".into()), Hp(500), Enemy));
}

fn output_players(players: Query<(&Name, &Hp)>) {
    println!("Current players:");

    for (name, hp) in &players {
        println!("Name: {}, HP: {}", name.0, hp.0);
    }

    println!();
}

// the code for remove_enemies
```

Output:

```text
Current players:
Name: Friend 1, HP: 100
Name: Friend 2, HP: 250
Name: Friend 3, HP: 150
Name: Enemy 1, HP: 300
Name: Enemy 2, HP: 500

Enemy 1 is dead.
Enemy 2 is dead.

Current players:
Name: Friend 1, HP: 100
Name: Friend 2, HP: 250
Name: Friend 3, HP: 150
```

<!-- :arrow_right:  Next: -->

:blue_book: Back: [Table of contents](./../README.md)

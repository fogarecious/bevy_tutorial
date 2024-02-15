# Removing Entities Directly

There is another way to remove an entity.
First, we get the id of the entity when the entity is spawned.
Then, we store the id somewhere (in another entity or in a resource).
After that, we retrieve the id and use the id to [despawn](https://docs.rs/bevy/latest/bevy/ecs/system/struct.EntityCommands.html#method.despawn) the entity.

```rust
#[derive(Component)]
struct Name(String);

#[derive(Component)]
struct Hp(u32);

#[derive(Component)]
struct MyTarget(Entity);

fn add_enemies(mut commands: Commands) {
    commands.spawn((Name("Enemy 1".into()), Hp(100)));

    let id = commands.spawn((Name("Enemy 2".into()), Hp(250))).id();
    commands.spawn(MyTarget(id));

    commands.spawn((Name("Enemy 3".into()), Hp(150)));
}

fn remove_my_target(mut commands: Commands, target_enemy: Query<&MyTarget>) {
    let id = target_enemy.single();

    commands.entity(id.0).despawn();
    println!("My target enemy is dead.");

    println!();
}
```

Here, the target entity that we want to remove is `Enemy 2`.
We use the [id](https://docs.rs/bevy/latest/bevy/ecs/system/struct.EntityCommands.html#method.id) method after the [spawn](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html#method.spawn) method to get the entity's id of type [Entity](https://docs.rs/bevy/latest/bevy/ecs/entity/struct.Entity.html).
We [spawn](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html#method.spawn) another entity `MyTarget` and store the id there.
The id is retrieved when we need to [despawn](https://docs.rs/bevy/latest/bevy/ecs/system/struct.EntityCommands.html#method.despawn) the entity in the `remove_my_target` system.

The full code is as follows:

```rust
use bevy::{
    app::{App, PostUpdate, PreUpdate, Startup, Update},
    ecs::{
        component::Component,
        entity::Entity,
        system::{Commands, Query},
    },
};

fn main() {
    App::new()
        .add_systems(Startup, add_enemies)
        .add_systems(PreUpdate, output_enemies)
        .add_systems(Update, remove_my_target)
        .add_systems(PostUpdate, output_enemies)
        .run();
}

// the code for the components: Name, Hp, and MyTarget

// the code for add_enemies and remove_my_target

fn output_enemies(enemies: Query<(&Name, &Hp)>) {
    println!("Current enemies:");

    for (name, hp) in &enemies {
        println!("Name: {}, HP: {}", name.0, hp.0);
    }

    println!();
}
```

Output:

```text
Current enemies:
Name: Enemy 1, HP: 100
Name: Enemy 2, HP: 250
Name: Enemy 3, HP: 150

My target enemy is dead.

Current enemies:
Name: Enemy 1, HP: 100
Name: Enemy 3, HP: 150

```

:arrow_right:  Next: [Too Many Type Parameters For Queries](./too_many_type_parameters_for_queries.md)

:blue_book: Back: [Table of contents](./../README.md)

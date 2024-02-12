# Searching And Updating Entities

Previously, we mentioned that we can use a system (like the following one) to iterate over all entities of type soldiers.

```rust
fn output_soldiers(soldiers: Query<(&Name, &Hp), Without<FinalBoss>>) {
    println!("Current soldiers:");

    for (name, hp) in &soldiers {
        println!("Name: {}, HP: {}", name.0, hp.0);
    }

    println!();
}
```

To update these entities, we can also use a similar system:

```rust
fn update_soldiers(mut soldiers: Query<&mut Hp, Without<FinalBoss>>) {
    for mut hp in &mut soldiers {
        hp.0 = 999;
    }
}
```

In the type parameters of [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html), we use `&mut` instead of `&`.
We also change the [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html) variable from immutable to mutable.
Then we use the mutable iterator of the [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html) variable to iterate over all requested entities as well as update their data.

In the case where there is exactly one requested entity:

```rust
fn output_boss(boss: Query<(&Name, &Hp), With<FinalBoss>>) {
    let (boss_name, boss_hp) = boss.single();
    println!("{}", boss_name.0);
    println!("HP: {}", boss_hp.0);
    println!();
}
```

We can use the [single_mut](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html#method.single_mut) method to update the entity.

```rust
fn update_boss(mut boss: Query<(&Name, &mut Hp), With<FinalBoss>>) {
    let (name, mut hp) = boss.single_mut();
    hp.0 = 0;
    println!("{} is dead.", name.0);
    println!();
}
```

Note that in the type parameters of [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html), we can combine immutable and mutable components.

For this kind of single entity, we can also use [get_single_mut](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html#method.get_single_mut) method of [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html) to avoid panics.

The full code is as follows:

```rust
use bevy::{
    app::{App, PostUpdate, PreUpdate, Startup, Update},
    ecs::{
        component::Component,
        query::{With, Without},
        schedule::IntoSystemConfigs,
        system::{Commands, Query},
    },
};

fn main() {
    App::new()
        .add_systems(Startup, add_players)
        .add_systems(PreUpdate, (output_soldiers, output_boss).chain())
        .add_systems(Update, (update_soldiers, update_boss))
        .add_systems(PostUpdate, (output_soldiers, output_boss).chain())
        .run();
}

#[derive(Component)]
struct Name(String);

#[derive(Component)]
struct Hp(u32);

#[derive(Component)]
struct FinalBoss;

fn add_players(mut commands: Commands) {
    commands.spawn((Name("Soldier 1".into()), Hp(100)));
    commands.spawn((Name("Soldier 2".into()), Hp(250)));
    commands.spawn((Name("Soldier 3".into()), Hp(150)));

    commands.spawn((Name("Final boss".into()), Hp(99999), FinalBoss));
}

// the code for output_soldiers, update_soldiers, output_boss, and update_boss
```

Output:

```text
Current soldiers:
Name: Soldier 1, HP: 100
Name: Soldier 2, HP: 250
Name: Soldier 3, HP: 150

Final boss
HP: 99999

Final boss is dead.

Current soldiers:
Name: Soldier 1, HP: 999
Name: Soldier 2, HP: 999
Name: Soldier 3, HP: 999

Final boss
HP: 0

```

<!-- :arrow_right:  Next: -->

:blue_book: Back: [Table of contents](./../README.md)

# Searching For The Only Entity

Usually, our queries return multiple entities, and we use a for loop to iterate over all the returned entities.
Sometimes, we are sure that a certain query has exactly one entity.
For example, we have exactly one entity that has a `FinalBoss` component in the following code.

```rust
use bevy::{
    app::{App, Startup, Update},
    ecs::{
        component::Component,
        query::With,
        system::{Commands, Query},
    },
};

fn main() {
    App::new()
        .add_systems(Startup, add_players)
        .add_systems(Update, output_boss)
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
```

In this case, we can replace the for loop with the [single](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html#method.single) method of [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html#):

```rust
fn output_boss(boss: Query<(&Name, &Hp), With<FinalBoss>>) {
    let (boss_name, boss_hp) = boss.single();
    println!("Name: {}, HP: {}", boss_name.0, boss_hp.0);
}
```

or the [get_single](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html#method.get_single) method:

```rust
fn output_boss(boss: Query<(&Name, &Hp), With<FinalBoss>>) {
    let boss_result = boss.get_single();
    match boss_result {
        Ok((boss_name, boss_hp)) => println!("Name: {}, HP: {}", boss_name.0, boss_hp.0),
        Err(_) => println!("The final boss is not here."),
    }
}
```

Output:

```text
Name: Final boss, HP: 99999
```

:arrow_right:  Next: [Searching And Updating Entities](./searching_and_updating_entities.md)

:blue_book: Back: [Table of contents](./../README.md)

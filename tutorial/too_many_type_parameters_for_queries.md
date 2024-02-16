# Too Many Type Parameters For Queries

Previously, we use the following system to print out the information about players.

```rust
fn output_players(players: Query<(&Name, &Hp)>) {
    println!("Current players:");

    for (name, hp) in &players {
        println!("Name: {}, HP: {}", name.0, hp.0);
    }

    println!();
}
```

This requires destructuring the [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html) parameter, i.e., we need to destructure an entity of `players` to be `(name, hp)`.
We have to make sure `(name, hp)` matches the order of the type parameters of `Query<(&Name, &Hp)>`.
In addition, if we need a lot of components of an entity, the list of the type parameters could be very long.

To solve this problem, we can use [WorldQuery](https://docs.rs/bevy/latest/bevy/ecs/query/derive.WorldQuery.html).

```rust
#[derive(WorldQuery)]
struct MyParameters {
    name: &'static Name,
    hp: &'static Hp,
}
```

We make a new struct that derives [WorldQuery](https://docs.rs/bevy/latest/bevy/ecs/query/derive.WorldQuery.html) and put all the type parameters originally declared in our [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html) into the struct.
Note that we have to explicitly define the lifetimes of the fields of references as `'static`.

Then we can use the [WorldQuery](https://docs.rs/bevy/latest/bevy/ecs/query/derive.WorldQuery.html) as a type parameter of [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html).

```rust
fn output_players(players: Query<MyParameters>) {
    println!("Current players:");

    for player in &players {
        println!("Name: {}, HP: {}", player.name.0, player.hp.0);
    }

    println!();
}
```

To have a mutable reference, we have to add `#[world_query(mutable)]` under `#[derive(WorldQuery)]` and change `&'static` to `&'static mut` for the corresponding fields.
In addition, a struct of [WorldQuery](https://docs.rs/bevy/latest/bevy/ecs/query/derive.WorldQuery.html) can also be a filter for a [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html).

```rust
#[derive(WorldQuery)]
#[world_query(mutable)]
struct MyMutParameters {
    name: &'static Name,
    hp: &'static mut Hp,
}

#[derive(WorldQuery)]
struct MyFilter {
    f1: With<Enemy>,
    f2: Without<FinalBoss>,
}

fn attack_normal_enemies(mut normal_enemies: Query<MyMutParameters, MyFilter>) {
    for mut enemy in &mut normal_enemies {
        enemy.hp.0 = 0;
        println!("{} is dead.", enemy.name.0);
    }

    println!();
}
```

The full code is as follows:

```rust
use bevy::{
    app::{App, PostUpdate, PreUpdate, Startup, Update},
    ecs::{
        component::Component,
        query::{With, Without, WorldQuery},
        system::{Commands, Query},
    },
};

fn main() {
    App::new()
        .add_systems(Startup, setup)
        .add_systems(PreUpdate, output_players)
        .add_systems(Update, attack_normal_enemies)
        .add_systems(PostUpdate, output_players)
        .run();
}

#[derive(Component)]
struct Name(String);

#[derive(Component)]
struct Hp(u32);

#[derive(Component)]
struct Enemy;

#[derive(Component)]
struct FinalBoss;

fn setup(mut commands: Commands) {
    commands.spawn((Name("Friend 1".into()), Hp(100)));
    commands.spawn((Name("Friend 2".into()), Hp(250)));
    commands.spawn((Name("Friend 3".into()), Hp(150)));

    commands.spawn((Name("Enemy 1".into()), Hp(300), Enemy));
    commands.spawn((Name("Enemy 2".into()), Hp(500), Enemy));

    commands.spawn((Name("Final boss".into()), Hp(99999), Enemy, FinalBoss));
}

// the code for MyParameters, MyMutParameters and MyFilter

// the code for output_players and attack_normal_enemies
```

Output:

```text
Current players:
Name: Friend 1, HP: 100
Name: Friend 2, HP: 250
Name: Friend 3, HP: 150
Name: Enemy 1, HP: 300
Name: Enemy 2, HP: 500
Name: Final boss, HP: 99999

Enemy 1 is dead.
Enemy 2 is dead.

Current players:
Name: Friend 1, HP: 100
Name: Friend 2, HP: 250
Name: Friend 3, HP: 150
Name: Enemy 1, HP: 0
Name: Enemy 2, HP: 0
Name: Final boss, HP: 99999

```

[WorldQuery](https://docs.rs/bevy/latest/bevy/ecs/query/derive.WorldQuery.html) is similar to [Bundle](https://docs.rs/bevy/latest/bevy/ecs/bundle/derive.Bundle.html), yet [Bundle](https://docs.rs/bevy/latest/bevy/ecs/bundle/derive.Bundle.html) is for spawning entities and [WorldQuery](https://docs.rs/bevy/latest/bevy/ecs/query/derive.WorldQuery.html) is for querying.

:arrow_right:  Next: [Too Many Parameters For Systems](./too_many_parameters_for_systems.md)

:blue_book: Back: [Table of contents](./../README.md)

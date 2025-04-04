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

To solve this problem, we can use [QueryData](https://docs.rs/bevy/latest/bevy/ecs/query/derive.QueryData.html).

```rust
#[derive(QueryData)]
struct MyParameters<'a> {
    name: &'a Name,
    hp: &'a Hp,
}
```

We make a new struct that derives [QueryData](https://docs.rs/bevy/latest/bevy/ecs/query/derive.QueryData.html) and put all the type parameters originally declared in our [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html) into the struct.
Note that we have to explicitly define the lifetimes of the fields of references.

Then we can use the [QueryData](https://docs.rs/bevy/latest/bevy/ecs/query/derive.QueryData.html) as a type parameter of [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html).

```rust
fn output_players(players: Query<MyParameters>) {
    println!("Current players:");

    for player in &players {
        println!("Name: {}, HP: {}", player.name.0, player.hp.0);
    }

    println!();
}
```

To have a mutable reference, we have to add `#[query_data(mutable)]` under `#[derive(QueryData)]` and change the corresponding fields to mutable references.
In addition, we can create a [QueryFilter](https://docs.rs/bevy/latest/bevy/ecs/query/derive.QueryFilter.html) for a [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html).

```rust
#[derive(QueryData)]
#[query_data(mutable)]
struct MyMutParameters<'a> {
    name: &'a Name,
    hp: &'a mut Hp,
}

#[derive(QueryFilter)]
struct MyFilter {
    with_enemy: With<Enemy>,
    without_boss: Without<FinalBoss>,
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
        query::{QueryData, QueryFilter, With, Without},
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

#[derive(QueryData)]
struct MyParameters<'a> {
    name: &'a Name,
    hp: &'a Hp,
}

#[derive(QueryData)]
#[query_data(mutable)]
struct MyMutParameters<'a> {
    name: &'a Name,
    hp: &'a mut Hp,
}

#[derive(QueryFilter)]
struct MyFilter {
    with_enemy: With<Enemy>,
    without_boss: Without<FinalBoss>,
}

fn setup(mut commands: Commands) {
    commands.spawn((Name("Friend 1".into()), Hp(100)));
    commands.spawn((Name("Friend 2".into()), Hp(250)));
    commands.spawn((Name("Friend 3".into()), Hp(150)));

    commands.spawn((Name("Enemy 1".into()), Hp(300), Enemy));
    commands.spawn((Name("Enemy 2".into()), Hp(500), Enemy));

    commands.spawn((Name("Final boss".into()), Hp(99999), Enemy, FinalBoss));
}

fn output_players(players: Query<MyParameters>) {
    println!("Current players:");

    for player in &players {
        println!("Name: {}, HP: {}", player.name.0, player.hp.0);
    }

    println!();
}

fn attack_normal_enemies(mut normal_enemies: Query<MyMutParameters, MyFilter>) {
    for mut enemy in &mut normal_enemies {
        enemy.hp.0 = 0;
        println!("{} is dead.", enemy.name.0);
    }

    println!();
}
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

[QueryData](https://docs.rs/bevy/latest/bevy/ecs/query/derive.QueryData.html) is similar to [Bundle](https://docs.rs/bevy/latest/bevy/ecs/bundle/derive.Bundle.html), yet [Bundle](https://docs.rs/bevy/latest/bevy/ecs/bundle/derive.Bundle.html) is for spawning entities and [QueryData](https://docs.rs/bevy/latest/bevy/ecs/query/derive.QueryData.html) is for querying.

:arrow_right:  Next: [Too Many Parameters For Systems](./too_many_parameters_for_systems.md)

:blue_book: Back: [Table of contents](./../README.md)

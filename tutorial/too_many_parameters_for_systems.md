# Too Many Parameters For Systems

Sometimes, it is the systems that have a long list of parameters.
For example, we may need to access entities and resources of different types in one system.

In this case, we use [SystemParam](https://docs.rs/bevy/latest/bevy/ecs/system/derive.SystemParam.html) to group parameters of a system.
In the following code, we have a struct `PlayerAction` that derives [SystemParam](https://docs.rs/bevy/latest/bevy/ecs/system/derive.SystemParam.html) and contains a [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html) and a [ResMut](https://docs.rs/bevy/latest/bevy/ecs/system/struct.ResMut.html).

```rust
#[derive(Component)]
struct Name(String);

#[derive(Component)]
struct Hp(u32);

#[derive(Component)]
struct Enemy;

#[derive(Resource)]
struct EnemyCounter(u32);

#[derive(SystemParam)]
struct PlayerAction<'w, 's> {
    enemies: Query<'w, 's, (&'static Name, &'static mut Hp), With<Enemy>>,
    counter: ResMut<'w, EnemyCounter>,
}
```

Similar to [WorldQuery](https://docs.rs/bevy/latest/bevy/ecs/query/derive.WorldQuery.html), the references in [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html) have lifetime `'static`, and are `mut` if they need to be mutable.
Yet, we have to connect the lifetimes (`'w` and `'s`) between [SystemParam](https://docs.rs/bevy/latest/bevy/ecs/system/derive.SystemParam.html) and [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html) (as well as [Res](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Res.html) and [ResMut](https://docs.rs/bevy/latest/bevy/ecs/system/struct.ResMut.html)).

A benefit of using [SystemParam](https://docs.rs/bevy/latest/bevy/ecs/system/derive.SystemParam.html) is that we can have methods for the structs deriving [SystemParam](https://docs.rs/bevy/latest/bevy/ecs/system/derive.SystemParam.html).
This helps us reusing code when we need it in multiple systems.

```rust
impl<'w, 's> PlayerAction<'w, 's> {
    fn attack_enemies(&mut self) {
        for (name, mut hp) in &mut self.enemies {
            hp.0 = 0;
            self.counter.0 -= 1;
            println!("{} is dead.", name.0);
        }

        println!();
    }

    fn enemy_information(&self) {
        println!("Enemy count:");
        println!("{}", self.counter.0);

        println!("Current enemies:");
        for (name, hp) in &self.enemies {
            println!("Name: {}, HP: {}", name.0, hp.0);
        }

        println!();
    }
}
```

(
  Note that `self.enemies` in the `enemy_information` method does not need to be mutable.
  We declare it as mutable for the `attack_enemies` method.
  In practice, we should avoid this kind of usage.
)

Then, we can use the [SystemParam](https://docs.rs/bevy/latest/bevy/ecs/system/derive.SystemParam.html) in systems.

```rust
fn enemy_information(player_action: PlayerAction) {
    player_action.enemy_information();
}

fn attack_enemies(mut player_action: PlayerAction) {
    player_action.attack_enemies();
}
```

The full code is as follows:

```rust
use bevy::{
    app::{App, PostUpdate, PreUpdate, Startup, Update},
    ecs::{
        component::Component,
        query::With,
        system::{Commands, Query, ResMut, Resource, SystemParam},
    },
};

fn main() {
    App::new()
        .insert_resource(EnemyCounter(2))
        .add_systems(Startup, setup)
        .add_systems(PreUpdate, enemy_information)
        .add_systems(Update, attack_enemies)
        .add_systems(PostUpdate, enemy_information)
        .run();
}

// the code for the components: Name, Hp and Enemy

// the code for the resource: EnemyCounter

fn setup(mut commands: Commands) {
    commands.spawn((Name("Friend 1".into()), Hp(100)));
    commands.spawn((Name("Friend 2".into()), Hp(250)));
    commands.spawn((Name("Friend 3".into()), Hp(150)));

    commands.spawn((Name("Enemy 1".into()), Hp(300), Enemy));
    commands.spawn((Name("Enemy 2".into()), Hp(500), Enemy));
}

// the code for the SystemParam: PlayerAction

// the code for the systems: enemy_information and attack_enemies
```

Output;

```text
Enemy count:
2
Current enemies:
Name: Enemy 1, HP: 300
Name: Enemy 2, HP: 500

Enemy 1 is dead.
Enemy 2 is dead.

Enemy count:
0
Current enemies:
Name: Enemy 1, HP: 0
Name: Enemy 2, HP: 0

```

:arrow_right:  Next: [Too Many Systems](./too_many_systems.md)

:blue_book: Back: [Table of contents](./../README.md)

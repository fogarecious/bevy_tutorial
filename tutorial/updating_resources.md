# Updating Resources

[Resource](https://docs.rs/bevy/latest/bevy/ecs/system/trait.Resource.html) can also be updated in systems.
Similar to [Res](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Res.html)\<MyResource>, we use [ResMut](https://docs.rs/bevy/latest/bevy/ecs/system/struct.ResMut.html)\<MyResource> as a parameter of a system to have a mutable MyResource, where MyResource is the name of our [Resource](https://docs.rs/bevy/latest/bevy/ecs/system/trait.Resource.html).
Then we can modify the MyResource in the system.
Modifying a parameter of type [ResMut](https://docs.rs/bevy/latest/bevy/ecs/system/struct.ResMut.html)\<...> is like modifying a reference.

Note: Do not forget to add `mut` before the parameter name of [ResMut](https://docs.rs/bevy/latest/bevy/ecs/system/struct.ResMut.html)\<...> types.

```rust
use bevy::{
    app::{App, Startup},
    ecs::{
        schedule::IntoSystemConfigs,
        system::{Res, ResMut, Resource},
    },
};

fn main() {
    App::new()
        .insert_resource(MyResource { value: 10 })
        .add_systems(Startup, (increase_value, output_value).chain())
        .run();
}

#[derive(Resource)]
struct MyResource {
    value: u32,
}

fn increase_value(mut r: ResMut<MyResource>) {
    r.value += 1;
}

fn output_value(r: Res<MyResource>) {
    println!("{}", r.value);
}
```

Output:

```text
11
```

:arrow_right:  Next: [Removing Resources](./removing_resources.md)

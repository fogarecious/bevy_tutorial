# Removing Resources

[Resources](https://docs.rs/bevy/latest/bevy/ecs/system/trait.Resource.html) can be removed after they are added to the [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html).
We use a parameter of type [Commands](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html) in systems to remove [Resources](https://docs.rs/bevy/latest/bevy/ecs/system/trait.Resource.html).

[Commands](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html) help us modifying our [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html) even after the [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html) is initialized.
Because [Commands](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html) modify the [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html), be sure to make it to be mutable.

Note that it is [Commands](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html) with `s` at the end of the word.

```rust
use bevy::{
    app::{App, Last, Startup},
    ecs::system::{Commands, Res, Resource},
};

fn main() {
    App::new()
        .insert_resource(MyResource { value: 10 })
        .add_systems(Startup, remove_my_resource)
        .add_systems(Last, output_value)
        .run();
}

#[derive(Resource)]
struct MyResource {
    value: u32,
}

fn remove_my_resource(mut commands: Commands) {
    commands.remove_resource::<MyResource>();
}

fn output_value(r: Res<MyResource>) {
    // Panic!
    println!("{}", r.value);
}
```

The execution of the code gets panic in the `output_value` system, since the `MyResource` has been removed.

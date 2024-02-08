# They Are Singleton Structs

Most of the time, we would like to access data of our structs when we are using systems.
If we only need one instance of such structs, we can use the [Resource](https://docs.rs/bevy/latest/bevy/ecs/system/trait.Resource.html) trait and add the [Resource](https://docs.rs/bevy/latest/bevy/ecs/system/trait.Resource.html) to our [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html).

To do so, we need to have a struct that derives the [Resource](https://docs.rs/bevy/latest/bevy/ecs/system/derive.Resource.html) macro.
We can use the [insert_resource](https://docs.rs/bevy/latest/bevy/app/struct.App.html#method.insert_resource) method of [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html) to add an instance of the struct.
Then we can access the struct in our systems by declaring a parameter [Res](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Res.html)\<MyResource> where MyResource is the name of the struct.

```rust
use bevy::{
    app::{App, Startup},
    ecs::system::{Res, Resource},
};

fn main() {
    App::new()
        .insert_resource(MyResource { value: 10 })
        .add_systems(Startup, output_value)
        .run();
}

#[derive(Resource)]
struct MyResource {
    value: u32,
}

fn output_value(r: Res<MyResource>) {
    println!("{}", r.value);
}
```

Output:

```text
10
```

If `MyResource` has implemented the [Default](https://doc.rust-lang.org/std/default/trait.Default.html) trait:

```rust
impl Default for MyResource {
    fn default() -> Self {
        Self { value: 10 }
    }
}
```

Then we can use the [init_resource](https://docs.rs/bevy/latest/bevy/app/struct.App.html#method.init_resource) method of [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html) to initialize the struct instead of calling [insert_resource](https://docs.rs/bevy/latest/bevy/app/struct.App.html#method.insert_resource).

```rust
init_resource::<MyResource>()
```

:arrow_right:  Next: [Updating Resources](./updating_resources.md)

:blue_book: Back: [Table of contents](./../README.md)

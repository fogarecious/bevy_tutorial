# Regular Polygons

We can also add a regular polygon to our app.

```rust
use bevy::{
    app::{App, Startup},
    asset::Assets,
    core_pipeline::core_2d::Camera2dBundle,
    ecs::system::{Commands, ResMut},
    prelude::default,
    render::mesh::{shape::RegularPolygon, Mesh},
    sprite::ColorMesh2dBundle,
    DefaultPlugins,
};

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_systems(Startup, setup)
        .run();
}

fn setup(mut commands: Commands, mut meshes: ResMut<Assets<Mesh>>) {
    commands.spawn(Camera2dBundle::default());

    commands.spawn(ColorMesh2dBundle {
        mesh: meshes.add(RegularPolygon::new(50., 5).into()).into(),
        ..default()
    });
}
```

The [new](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.RegularPolygon.html#method.new) function of [RegularPolygon](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.RegularPolygon.html) accepts two parameters, which are the radius and the number of sides of the [RegularPolygon](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.RegularPolygon.html).

Result:

![Regular Polygons](./pic/regular_polygons.png)

<!-- :arrow_right:  Next:  -->

:blue_book: Back: [Table of contents](./../README.md)

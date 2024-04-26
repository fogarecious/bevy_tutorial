# Box

A [Box](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Box.html) is a cube with different lengths at each axis.

```rust
commands.spawn(PbrBundle {
    mesh: meshes.add(Box::new(2., 1., 0.5).into()).into(),
    ..default()
});
```

We use the function [new](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Box.html#method.new) of [Box](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Box.html) to construct a box.
The function takes three parameters.
They are the lengths of the box for x, y and z axis.

We set our camera position to `(2, 1, 3)` and make it looking at the origin.

The full code is as follows:

```rust
use bevy::{
    app::{App, Startup},
    asset::Assets,
    core_pipeline::core_3d::Camera3dBundle,
    ecs::system::{Commands, ResMut},
    math::Vec3,
    pbr::{PbrBundle, PointLightBundle, StandardMaterial},
    render::mesh::{shape::Box, Mesh},
    transform::components::Transform,
    utils::default,
    DefaultPlugins,
};

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_systems(Startup, setup)
        .run();
}

fn setup(
    mut commands: Commands,
    mut meshes: ResMut<Assets<Mesh>>,
    mut materials: ResMut<Assets<StandardMaterial>>,
) {
    commands.spawn(Camera3dBundle {
        transform: Transform::from_xyz(2., 1., 3.).looking_at(Vec3::ZERO, Vec3::Y),
        ..default()
    });

    commands.spawn(PbrBundle {
        mesh: meshes.add(Box::new(2., 1., 0.5).into()).into(),
        material: materials.add(StandardMaterial::default()).into(),
        ..default()
    });

    commands.spawn(PointLightBundle {
        transform: Transform::from_xyz(2., 2., 1.),
        ..default()
    });
}
```

Result:

![Box](./pic/box.png)

<!-- :arrow_right:  Next:  -->

:blue_book: Back: [Table of contents](./../README.md)

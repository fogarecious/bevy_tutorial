# Plane

[Plane](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Plane.html) is a thin box that usually acts as a floor or ceiling.

```rust
commands.spawn(PbrBundle {
    mesh: meshes.add(Plane::from_size(1.).into()).into(),
    ..default()
});
```

We use the function [from_size](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Plane.html#method.from_size) of [Plane](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Plane.html) to construct a [Plane](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Plane.html).
The [Plane](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Plane.html) is at the x-z plane by default.

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
    render::mesh::{shape::Plane, Mesh},
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
        mesh: meshes.add(Plane::from_size(1.).into()).into(),
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

![Plane](./pic/plane.png)

:arrow_right:  Next: [3D Transformation](./3d_transformation.md)

:blue_book: Back: [Table of contents](./../README.md)

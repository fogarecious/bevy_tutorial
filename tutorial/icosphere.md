# Icosphere

In addition to [UVSphere](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.UVSphere.html), another sphere is [Icosphere](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Icosphere.html).

Similar to [UVSphere](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.UVSphere.html), we initialize an [Icosphere](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Icosphere.html) and set its [radius](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Icosphere.html#structfield.radius).

```rust
commands.spawn(PbrBundle {
    mesh: meshes
        .add(
            Icosphere {
                radius: 0.5,
                ..default()
            }
            .try_into()
            .unwrap(),
        )
        .into(),
    ..default()
});
```

The difference between the two spheres is that, in [Icosphere](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Icosphere.html), we have to use [try_into()](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Icosphere.html#impl-TryFrom%3CIcosphere%3E-for-Mesh) and [unwrap()](https://doc.rust-lang.org/std/result/enum.Result.html#method.unwrap) instead of [into()](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.UVSphere.html#impl-From%3CUVSphere%3E-for-Mesh).
An error might appear if there are too many [subdivisions](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Icosphere.html#structfield.subdivisions) for an [Icosphere](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Icosphere.html), say more than 80.

We set our camera position to `(0, 0, 3)` and make it looking at the origin.

The full code is as follows:

```rust
use bevy::{
    app::{App, Startup},
    asset::Assets,
    core_pipeline::core_3d::Camera3dBundle,
    ecs::system::{Commands, ResMut},
    math::Vec3,
    pbr::{PbrBundle, PointLightBundle, StandardMaterial},
    render::mesh::{shape::Icosphere, Mesh},
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
        transform: Transform::from_xyz(0., 0., 3.).looking_at(Vec3::ZERO, Vec3::Y),
        ..default()
    });

    commands.spawn(PbrBundle {
        mesh: meshes
            .add(
                Icosphere {
                    radius: 0.5,
                    ..default()
                }
                .try_into()
                .unwrap(),
            )
            .into(),
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

![Icosphere](./pic/icosphere.png)

:arrow_right:  Next: [Cylinder](./cylinder.md)

:blue_book: Back: [Table of contents](./../README.md)

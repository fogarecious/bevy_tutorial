# UVSphere

A sphere is a ball-shape object.

One of the spheres in [Bevy](https://bevyengine.org/) is [UVSphere](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.UVSphere.html).

```rust
commands.spawn(PbrBundle {
    mesh: meshes
        .add(
            UVSphere {
                radius: 0.5,
                ..default()
            }
            .into(),
        )
        .into(),
    ..default()
});
```

We can set the [radius](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.UVSphere.html#structfield.radius) of the [UVSphere](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.UVSphere.html).

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
    render::mesh::{shape::UVSphere, Mesh},
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
                UVSphere {
                    radius: 0.5,
                    ..default()
                }
                .into(),
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

![UVSphere](./pic/uvsphere.png)

:arrow_right:  Next: [Icosphere](./icosphere.md)

:blue_book: Back: [Table of contents](./../README.md)

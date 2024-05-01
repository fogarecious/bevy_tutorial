# Capsule

[Capsule](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Capsule.html) is like two spheres connected by a cylinder.

```rust
commands.spawn(PbrBundle {
    mesh: meshes
        .add(
            Capsule {
                radius: 0.5,
                depth: 0.5,
                ..default()
            }
            .into(),
        )
        .into(),
    ..default()
});
```

We can set its [radius](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Capsule.html#structfield.radius) and [depth](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Capsule.html#structfield.depth) (for the height of the cylinder).

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
    render::mesh::{shape::Capsule, Mesh},
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
                Capsule {
                    radius: 0.5,
                    depth: 0.5,
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

![Capsule](./pic/capsule.png)

<!-- :arrow_right:  Next:  -->

:blue_book: Back: [Table of contents](./../README.md)

# Spot Light

Spot light starts from a point, follows a direction and creates a light of a cone shape.

We use [SpotLightBundle](https://docs.rs/bevy/latest/bevy/pbr/struct.SpotLightBundle.html) to create a spot light.

```rust
commands.spawn(SpotLightBundle {
    transform: Transform::from_xyz(0., 2., 0.).looking_at(Vec3::new(0., -1., 0.), Vec3::Z),
    ..default()
});
```

In [transform](https://docs.rs/bevy/latest/bevy/pbr/struct.SpotLightBundle.html#structfield.transform) of [SpotLightBundle](https://docs.rs/bevy/latest/bevy/pbr/struct.SpotLightBundle.html), we use [from_xyz](https://docs.rs/bevy/latest/bevy/transform/components/struct.Transform.html#method.from_xyz) and [looking_at](https://docs.rs/bevy/latest/bevy/transform/components/struct.Transform.html#method.looking_at) to set the start position and direction of the light respectively.
In the example, the light starts from the position `(0, 2, 0)` and faces to the direction `(0, -1, 0)`.

We place a [Plane](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Plane.html) to indicate the x-z plane and put a [Cube](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Cube.html) on it.
We set our camera position to `(2, 2, 3)` and make it looking at the origin.

The full code is as follows:

```rust
use bevy::{
    app::{App, Startup},
    asset::Assets,
    core_pipeline::core_3d::Camera3dBundle,
    ecs::system::{Commands, ResMut},
    math::Vec3,
    pbr::{PbrBundle, SpotLightBundle, StandardMaterial},
    render::mesh::{
        shape::{Cube, Plane},
        Mesh,
    },
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
        transform: Transform::from_xyz(2., 2., 3.).looking_at(Vec3::ZERO, Vec3::Y),
        ..default()
    });

    commands.spawn(PbrBundle {
        mesh: meshes.add(Cube::new(1.).into()).into(),
        transform: Transform::from_xyz(0., 0.5, 0.),
        material: materials.add(StandardMaterial::default()).into(),
        ..default()
    });

    commands.spawn(PbrBundle {
        mesh: meshes.add(Plane::from_size(5.).into()).into(),
        material: materials.add(StandardMaterial::default()).into(),
        ..default()
    });

    commands.spawn(SpotLightBundle {
        transform: Transform::from_xyz(0., 2., 0.).looking_at(Vec3::new(0., -1., 0.), Vec3::Z),
        ..default()
    });
}
```

Result:

![Spot Light](./pic/spot_light.png)

:arrow_right:  Next: [Light Colors](./light_colors.md)

:blue_book: Back: [Table of contents](./../README.md)

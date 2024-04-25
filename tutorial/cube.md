# Cube

Similar to 2D rendering, we can add shapes into the 3D space.
This is also done by spawning a bundle, adding a shape to the mesh asset and adding a material to some material asset.
The shapes can be found in [bevy::prelude::shape](https://docs.rs/bevy/latest/bevy/prelude/shape/index.html).

In the following example, we add a cube to the 3D scene.

We make the app to spawn a [PbrBundle](https://docs.rs/bevy/latest/bevy/pbr/type.PbrBundle.html).

```rust
commands.spawn(PbrBundle {
    mesh: meshes.add(Cube::new(1.).into()).into(),
    ..default()
});
```

We add a [Cube](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Cube.html) to the [PbrBundle](https://docs.rs/bevy/latest/bevy/pbr/type.PbrBundle.html).
The [Cube](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Cube.html) is also added to the mesh asset.
We use the function [new](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Cube.html#method.new) of [Cube](https://docs.rs/bevy/latest/bevy/prelude/shape/struct.Cube.html) to initialize the cube.
The function takes a parameter `size`, which defines the size of the cube.

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
    render::mesh::{shape::Cube, Mesh},
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
        mesh: meshes.add(Cube::new(1.).into()).into(),
        material: materials.add(StandardMaterial::default()).into(),
        ..default()
    });

    commands.spawn(PointLightBundle {
        transform: Transform::from_xyz(2., 2., 1.),
        ..default()
    });
}
```

Currently, we omit the explanation of `StandardMaterial` and `PointLightBundle`.
They will be explained later.
Basically, we make the cube visible by the camera.

![Cube](./pic/cube.png)

<!-- :arrow_right:  Next:  -->

:blue_book: Back: [Table of contents](./../README.md)

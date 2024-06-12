# Moonlight

We can use fog to make special light, such as moonlight or sunlight.

To create moonlight, we use [FogFalloff::Atmospheric](https://docs.rs/bevy/latest/bevy/pbr/enum.FogFalloff.html#variant.Atmospheric) in [FogSettings](https://docs.rs/bevy/latest/bevy/pbr/struct.FogSettings.html).

```rust
commands.spawn((
    Camera3dBundle::default(),
    FogSettings {
        color: Color::rgb(0.25, 0.25, 0.25),
        falloff: FogFalloff::from_visibility_color(3., Color::BLUE),
        ..default()
    },
));
```

To construct [FogFalloff::Atmospheric](https://docs.rs/bevy/latest/bevy/pbr/enum.FogFalloff.html#variant.Atmospheric), we can use [FogFalloff::from_visibility_color](https://docs.rs/bevy/latest/bevy/pbr/enum.FogFalloff.html#method.from_visibility_color), which takes a visibility and a color as its two parameters.
The visibility parameter is a distance that the moonlight works in.
The color parameter is the color of the moonlight.

The [color](https://docs.rs/bevy/latest/bevy/pbr/struct.FogSettings.html#structfield.color) of [FogSettings](https://docs.rs/bevy/latest/bevy/pbr/struct.FogSettings.html) controls how strong the moonlight is.
We use a lower brightness in the example to create a weaker moonlight.

The full code is as follows:

```rust
use bevy::{
    app::{App, Startup},
    asset::Assets,
    core_pipeline::core_3d::Camera3dBundle,
    ecs::system::{Commands, ResMut},
    math::Vec3,
    pbr::{
        DirectionalLight, DirectionalLightBundle, FogFalloff, FogSettings, NotShadowCaster,
        PbrBundle, StandardMaterial,
    },
    render::{
        color::Color,
        mesh::{
            shape::{Plane, UVSphere},
            Mesh,
        },
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
    commands.spawn((
        Camera3dBundle {
            transform: Transform::from_xyz(2., 1., 2.).looking_at(Vec3::new(0., 0.5, 0.), Vec3::Y),
            ..default()
        },
        FogSettings {
            color: Color::rgb(0.25, 0.25, 0.25),
            falloff: FogFalloff::from_visibility_color(3., Color::BLUE),
            ..default()
        },
    ));

    // left
    commands.spawn(PbrBundle {
        mesh: meshes.add(
            UVSphere {
                radius: 0.5,
                ..default()
            }
            .into(),
        ),
        material: materials.add(StandardMaterial {
            base_color: Color::RED,
            ..default()
        }),
        transform: Transform::from_xyz(-1.25, 0.5, 0.),
        ..default()
    });

    // middle
    commands.spawn(PbrBundle {
        mesh: meshes.add(
            UVSphere {
                radius: 0.5,
                ..default()
            }
            .into(),
        ),
        material: materials.add(StandardMaterial {
            base_color: Color::GREEN,
            ..default()
        }),
        transform: Transform::from_xyz(0., 0.5, 0.),
        ..default()
    });

    // right
    commands.spawn(PbrBundle {
        mesh: meshes.add(
            UVSphere {
                radius: 0.5,
                ..default()
            }
            .into(),
        ),
        material: materials.add(StandardMaterial {
            base_color: Color::BLUE,
            ..default()
        }),
        transform: Transform::from_xyz(1.25, 0.5, 0.),
        ..default()
    });

    // ground
    commands.spawn(PbrBundle {
        mesh: meshes.add(Plane::from_size(5.).into()),
        material: materials.add(StandardMaterial::default()),
        ..default()
    });

    // sky
    commands.spawn((
        PbrBundle {
            mesh: meshes.add(
                UVSphere {
                    radius: 10.,
                    ..default()
                }
                .into(),
            ),
            material: materials.add(StandardMaterial {
                cull_mode: None,
                ..default()
            }),
            ..default()
        },
        NotShadowCaster,
    ));

    // light
    commands.spawn(DirectionalLightBundle {
        directional_light: DirectionalLight {
            illuminance: 20000.,
            shadows_enabled: true,
            ..default()
        },
        transform: Transform::default().looking_to(Vec3::new(-1., -3., 0.), Vec3::Y),
        ..default()
    });
}
```

Result:

![Moonlight](./pic/moonlight.png)

<!-- :arrow_right:  Next:  -->

:blue_book: Back: [Table of contents](./../README.md)

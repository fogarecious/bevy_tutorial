# Camera 3D

Similar to 2D rendering, which needs [Camera2dBundle](https://docs.rs/bevy/latest/bevy/core_pipeline/core_2d/struct.Camera2dBundle.html), 3D rendering needs [Camera3dBundle](https://docs.rs/bevy/latest/bevy/core_pipeline/core_3d/struct.Camera3dBundle.html) in order to show any 3D objects.

```rust
use bevy::{
    app::{App, Startup},
    core_pipeline::core_3d::Camera3dBundle,
    ecs::system::Commands,
    DefaultPlugins,
};

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_systems(Startup, setup)
        .run();
}

fn setup(mut commands: Commands) {
    commands.spawn(Camera3dBundle::default());
}
```

In default, the camera is at the position `(0, 0, 0)`.
The 3D space is consisted of the x-axis, y-axis and z-axis.
Positive x points to right.
Positive y points to up.
And positive z points out of the screen, toward us.

<!-- :arrow_right:  Next:  -->

:blue_book: Back: [Table of contents](./../README.md)

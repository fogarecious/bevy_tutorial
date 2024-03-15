# Camera 2D

To draw 2-dimensional objects including images and any kind of shapes, we need to [spawn](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html#method.spawn) a [Camera2dBundle](https://docs.rs/bevy/latest/bevy/core_pipeline/core_2d/struct.Camera2dBundle.html).

```rust
use bevy::{
    app::{App, Startup},
    core_pipeline::core_2d::Camera2dBundle,
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
    commands.spawn(Camera2dBundle::default());
}
```

The [Camera2dBundle](https://docs.rs/bevy/latest/bevy/core_pipeline/core_2d/struct.Camera2dBundle.html) contains a [Camera](https://docs.rs/bevy/latest/bevy/render/camera/struct.Camera.html) component.
The component decides where to draw 2-dimensional objects, such as on which window and on which rectangular area of the window.
The default is to draw on the whole primary window.

:arrow_right:  Next: [The Background Color](./the_background_color.md)

:blue_book: Back: [Table of contents](./../README.md)

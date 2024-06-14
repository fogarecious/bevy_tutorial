# Skipping The White Window

When we start a new window, we often see a white window before the scene is ready.
This white window remains for a longer time if we have a 3D scene.
We can play a trick and skip (or hide) the white window.

First, we hide the window.
(See [Initializing A Different Window](./initializing_a_different_window.md) for how to change window.)
We set the visible of the window to `false`.

```rust
App::new()
    .add_plugins(DefaultPlugins.set(WindowPlugin {
        primary_window: Some(Window {
            visible: false,
            ..default()
        }),
        ..default()
    }))
```

When the GPU is ready, we set the visible of the window back to `true`.

```rust
fn make_visible(mut window: Query<&mut Window>, frame_count: Res<FrameCount>) {
    if frame_count.0 == 3 {
        window.single_mut().visible = true;
    }
}
```

We use a resource [FrameCount](https://docs.rs/bevy/latest/bevy/core/struct.FrameCount.html).
Usually, the GPU will be ready after 3 frames.

The full code is as follows:

```rust
use bevy::app::{PluginGroup, Update};
use bevy::core::FrameCount;
use bevy::ecs::system::{Query, Res};
use bevy::utils::default;
use bevy::window::{Window, WindowPlugin};
use bevy::{app::App, DefaultPlugins};

fn main() {
    App::new()
        .add_plugins(DefaultPlugins.set(WindowPlugin {
            primary_window: Some(Window {
                visible: false,
                ..default()
            }),
            ..default()
        }))
        .add_systems(Update, make_visible)
        .run();
}

fn make_visible(mut window: Query<&mut Window>, frame_count: Res<FrameCount>) {
    if frame_count.0 == 3 {
        window.single_mut().visible = true;
    }
}
```

By running the program, we can see that everything is ready when the window appears.

<!-- :arrow_right:  Next:  -->

:blue_book: Back: [Table of contents](./../README.md)

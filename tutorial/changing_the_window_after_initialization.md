# Changing The Window After Initialization

When we add the [DefaultPlugins](https://docs.rs/bevy/latest/bevy/struct.DefaultPlugins.html), it adds the [WindowPlugin](https://docs.rs/bevy/latest/bevy/window/struct.WindowPlugin.html) to our [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html).
The [WindowPlugin](https://docs.rs/bevy/latest/bevy/window/struct.WindowPlugin.html) contains a [Window](https://docs.rs/bevy/latest/bevy/window/struct.Window.html) struct that defines the appearance of our window.
The [Window](https://docs.rs/bevy/latest/bevy/window/struct.Window.html) can be changed even the [WindowPlugin](https://docs.rs/bevy/latest/bevy/window/struct.WindowPlugin.html) has been initialized.

To change the window, we can query the [Window](https://docs.rs/bevy/latest/bevy/window/struct.Window.html) in a system.

```rust
use bevy::{
    app::{App, PostStartup},
    ecs::system::Query,
    window::{Window, WindowPosition},
    DefaultPlugins,
};

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_systems(PostStartup, change_window)
        .run();
}

fn change_window(mut windows: Query<&mut Window>) {
    let mut window = windows.single_mut();

    window.title = "A Bevy App".into();
    window.position = WindowPosition::At((0, 0).into());
    window.resolution = (200., 100.).into();
}
```

We schedule the `change_window` system on [PostStartup](https://docs.rs/bevy/latest/bevy/app/struct.PostStartup.html) for simplicity.
The system can also be called anytime later, such as key inputs or timers.

We use `windows.single_mut()` to get the [Window](https://docs.rs/bevy/latest/bevy/window/struct.Window.html) because we are sure that there is exactly one window here.

Result:

![Changing The Window After Initialization](./pic/changing_the_window_after_initialization.png)

<!-- :arrow_right:  Next:  -->

:blue_book: Back: [Table of contents](./../README.md)

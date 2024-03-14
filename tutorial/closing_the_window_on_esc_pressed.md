# Closing The Window On Esc Pressed

There are build-in systems in [Bevy](https://bevyengine.org/) that increase our convenience.
For example, the system [close_on_esc](https://docs.rs/bevy/latest/bevy/window/fn.close_on_esc.html) will close the window when the Esc key is pressed.

```rust
use bevy::{
    app::{App, Update},
    window::close_on_esc,
    DefaultPlugins,
};

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_systems(Update, close_on_esc)
        .run();
}
```

The [close_on_esc](https://docs.rs/bevy/latest/bevy/window/fn.close_on_esc.html) is added to the schedule [Update](https://docs.rs/bevy/latest/bevy/app/struct.Update.html), so that our [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html) checks if the Esc key is pressed whenever the app is updated.

:arrow_right:  Next: [Camera 2D](./camera_2d.md)

:blue_book: Back: [Table of contents](./../README.md)

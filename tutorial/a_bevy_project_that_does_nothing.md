# A Bevy Project That Does Nothing

Any [Bevy](https://bevyengine.org/) app starts from the [bevy::app::App](https://docs.rs/bevy/latest/bevy/app/struct.App.html) struct.
We initialize the [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html) struct by its [new()](https://docs.rs/bevy/latest/bevy/app/struct.App.html#method.new) function, and call the [run()](https://docs.rs/bevy/latest/bevy/app/struct.App.html#method.run) method to start the app.

```rust
use bevy::app::App;

fn main() {
    App::new().run();
}
```

This program terminates immediately and outputs nothing.
Yet, it is the template of all [Bevy](https://bevyengine.org/) apps that we can add functions and features to.

:arrow_right:  Next: [They Are Functions In Bevy](./they_are_functions_in_bevy.md)

# Triggering An Event

Previously, we introduced how to use [KeyboardInput](https://docs.rs/bevy/latest/bevy/input/keyboard/struct.KeyboardInput.html) and [CursorMoved](https://docs.rs/bevy/latest/bevy/window/struct.CursorMoved.html) to monitor the keyboard and the mouse.
In fact, [KeyboardInput](https://docs.rs/bevy/latest/bevy/input/keyboard/struct.KeyboardInput.html) and [CursorMoved](https://docs.rs/bevy/latest/bevy/window/struct.CursorMoved.html) are [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html)s.
We can not only receive [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html)s but also send [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html)s, where the latter can be done by [EventWriter](https://docs.rs/bevy/latest/bevy/ecs/event/struct.EventWriter.html).

In the following example, we show how to exit the app by pressing the key `space`.
Exiting the app is done by sending an [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html) of [AppExit](https://docs.rs/bevy/latest/bevy/app/struct.AppExit.html).

```rust
fn handle_keys(keyboard_input: Res<Input<KeyCode>>, mut events: EventWriter<AppExit>) {
    if keyboard_input.just_pressed(KeyCode::Space) {
        events.send(AppExit);
    }
}
```

In the system `handle_keys`, we consider a parameter [EventWriter](https://docs.rs/bevy/latest/bevy/ecs/event/struct.EventWriter.html)<[AppExit](https://docs.rs/bevy/latest/bevy/app/struct.AppExit.html)>.
We use the method [send](https://docs.rs/bevy/latest/bevy/ecs/event/struct.EventWriter.html#method.send) (with [AppExit](https://docs.rs/bevy/latest/bevy/app/struct.AppExit.html)) of [EventWriter](https://docs.rs/bevy/latest/bevy/ecs/event/struct.EventWriter.html)<[AppExit](https://docs.rs/bevy/latest/bevy/app/struct.AppExit.html)> to trigger an [AppExit](https://docs.rs/bevy/latest/bevy/app/struct.AppExit.html) event.
The [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html) will exit when it receives an [AppExit](https://docs.rs/bevy/latest/bevy/app/struct.AppExit.html) event.

The full code is as follows:

```rust
use bevy::{
    app::{App, AppExit, Update},
    ecs::{event::EventWriter, system::Res},
    input::{keyboard::KeyCode, Input},
    DefaultPlugins,
};

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_systems(Update, handle_keys)
        .run();
}

fn handle_keys(keyboard_input: Res<Input<KeyCode>>, mut events: EventWriter<AppExit>) {
    if keyboard_input.just_pressed(KeyCode::Space) {
        events.send(AppExit);
    }
}
```

By running the program, we can see that there is a window at the beginning.
The window is closed when we press the key `space`.
And also the app exits when the window is closed.

:arrow_right:  Next: [Custom Events](./custom_events.md)

:blue_book: Back: [Table of contents](./../README.md)

# Custom Events

In addition to the build-in [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html)s, we can create our own [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html)s.

In the following example, we will trigger our own [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html) when we press the key `space`.
This [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html) will be received by another system, where we will print the content of the [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html).

To create an [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html), we make a struct to derive [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html).

```rust
#[derive(Event)]
struct MyEvent(u32);
```

`MyEvent` contains an unsigned 32-bit integer as its content.

This [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html) must be added to the [App](https://docs.rs/bevy/latest/bevy/app/struct.App.html).

```rust
App::new().add_event::<MyEvent>()
```

To trigger the [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html), we include the system parameter [EventWriter](https://docs.rs/bevy/latest/bevy/ecs/event/struct.EventWriter.html)<`MyEvent`>.

```rust
fn handle_keys(keyboard_input: Res<Input<KeyCode>>, mut events: EventWriter<MyEvent>) {
    if keyboard_input.just_pressed(KeyCode::Space) {
        events.send(MyEvent(99));
    }
}
```

We use the method [send](https://docs.rs/bevy/latest/bevy/ecs/event/struct.EventWriter.html#method.send) of [EventWriter](https://docs.rs/bevy/latest/bevy/ecs/event/struct.EventWriter.html)<`MyEvent`> to send the [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html).

To receive the [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html), we refer to the system parameter [EventReader](https://docs.rs/bevy/latest/bevy/ecs/event/struct.EventReader.html)<`MyEvent`>.

```rust
fn handle_my_events(mut events: EventReader<MyEvent>) {
    for my_event in events.read() {
        println!("{}", my_event.0);
    }
}
```

We use the method [read](https://docs.rs/bevy/latest/bevy/ecs/event/struct.EventReader.html#method.read) of [EventReader](https://docs.rs/bevy/latest/bevy/ecs/event/struct.EventReader.html)<`MyEvent`> to receive the [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html).
Then we print the content of the [Event](https://docs.rs/bevy/latest/bevy/ecs/event/trait.Event.html).

The full code is as follows:

```rust
use bevy::{
    app::{App, Update},
    ecs::{
        event::{Event, EventReader, EventWriter},
        system::Res,
    },
    input::{keyboard::KeyCode, Input},
    DefaultPlugins,
};

#[derive(Event)]
struct MyEvent(u32);

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_event::<MyEvent>()
        .add_systems(Update, (handle_keys, handle_my_events))
        .run();
}

fn handle_keys(keyboard_input: Res<Input<KeyCode>>, mut events: EventWriter<MyEvent>) {
    if keyboard_input.just_pressed(KeyCode::Space) {
        events.send(MyEvent(99));
    }
}

fn handle_my_events(mut events: EventReader<MyEvent>) {
    for my_event in events.read() {
        println!("{}", my_event.0);
    }
}
```

When we press the key `space`, we get the output:

```text
99
```

:arrow_right:  Next: [Turning On/Off A System](./turning_on_off_a_system.md)

:blue_book: Back: [Table of contents](./../README.md)

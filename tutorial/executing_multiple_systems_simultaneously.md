# Executing Multiple Systems Simultaneously

One of the power of [Bevy](https://bevyengine.org/) is to run multiple systems simultaneously.
To do so, we put multiple systems in a pair of parentheses and pass them to the [add_systems](https://docs.rs/bevy/latest/bevy/app/struct.App.html#method.add_systems) method.

```rust
use bevy::app::{App, Startup};

fn main() {
    App::new().add_systems(Startup, (hello_a, hello_b)).run();
}

fn hello_a() {
    println!("Hello A!");
}

fn hello_b() {
    println!("Hello B!");
}
```

Since the two systems run in parallel, there is no guarantee which one would be executed first.
The output could be either

```text
Hello A!
Hello B!
```

or

```text
Hello B!
Hello A!
```

To test these systems and see if they really run in parallel, we can make them [sleep](https://doc.rust-lang.org/std/thread/fn.sleep.html) for a while.

```rust
use std::{thread, time::Duration};

// ...

fn hello_a() {
    thread::sleep(Duration::from_secs(1));
    println!("Hello A!");
}

fn hello_b() {
    thread::sleep(Duration::from_secs(1));
    println!("Hello B!");
}
```

We will see `Hello A!` and `Hello B!` appears at the same time.

The number of systems in a pair of parentheses is limited to 15.
Yet, we can have more parentheses inside.

```rust
add_systems(Startup, (hello_a, (hello_b, hello_c)))
```

The added parentheses can be recursive, so the number of systems we can have can be treated as unlimited conceptually.

:arrow_right:  Next: [Executing Multiple Systems In Order](./executing_multiple_systems_in_order.md)

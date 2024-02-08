# Executing Multiple Systems In Order

Systems can also be run sequentially.
To do so, we use the [chain](https://docs.rs/bevy/latest/bevy/ecs/prelude/trait.IntoSystemConfigs.html#method.chain) method of [IntoSystemConfigs](https://docs.rs/bevy/latest/bevy/ecs/prelude/trait.IntoSystemConfigs.html).
More precisely, we put multiple systems in a pair of parentheses and call the [chain](https://docs.rs/bevy/latest/bevy/ecs/prelude/trait.IntoSystemConfigs.html#method.chain) method of the parentheses.

```rust
use bevy::{
    app::{App, Startup},
    ecs::schedule::IntoSystemConfigs,
};

fn main() {
    App::new()
        .add_systems(Startup, (hello_a, hello_b, hello_c).chain())
        .run();
}

fn hello_a() {
    println!("Hello A!");
}

fn hello_b() {
    println!("Hello B!");
}

fn hello_c() {
    println!("Hello C!");
}
```

Output:

```text
Hello A!
Hello B!
Hello C!
```

In addition to [chain](https://docs.rs/bevy/latest/bevy/ecs/prelude/trait.IntoSystemConfigs.html#method.chain), we can also use the [before](https://docs.rs/bevy/latest/bevy/ecs/prelude/trait.IntoSystemConfigs.html#method.before) or [after](https://docs.rs/bevy/latest/bevy/ecs/prelude/trait.IntoSystemConfigs.html#method.after) methods.
All these methods work on an individual system (a function) and a set of systems (the parentheses containing multiple functions).

```rust
add_systems(Startup, (hello_a, hello_b.after(hello_a), hello_c))
```

There are three possible outputs of the code above:

```text
Hello A!
Hello B!
Hello C!
```

```text
Hello A!
Hello C!
Hello B!
```

or

```text
Hello C!
Hello A!
Hello B!
```

:arrow_right:  Next: [Executing Multiple Systems Set By Set](./executing_multiple_systems_set_by_set.md)

:blue_book: Back: [Table of contents](./../README.md)
